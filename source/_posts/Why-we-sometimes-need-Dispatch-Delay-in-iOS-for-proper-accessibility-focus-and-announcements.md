---
title: >-
  Why we sometimes need Dispatch Delay in iOS for proper accessibility focus and
  announcements?
readtime: 10
thumbnail: >-
  /Why we sometimes need Dispatch Delay in iOS for proper accessibility focus
  and announcements?/header.png
date: 2025-03-27 23:55:58
description: This article explains why Dispatch Delay is sometimes necessary in iOS to ensure proper VoiceOver focus and announcements. It covers common accessibility issues, how timing affects VoiceOver behavior, and when delaying updates can improve the user experience.
tags:
  - Swift
  - Accessibility
  - iOS
  - UIKit
---

In iOS development, if you want to make your app inclusive, creating an accessible experience for users with disabilities is a key priority. For users relying on assistive technologies like VoiceOver, it’s important to properly refocus on UI elements and ensure that important announcements are made. Refocusing allows users to navigate efficiently, while timely announcements ensure they receive critical information about changes in the app. One issue commonly faced by developers while working with such challenges is simply updating the layout or announcing text, does not always work as intended, sometimes. This can lead to missed or delayed notifications for users. In this article, we’ll discuss why such issues occur and how to properly handle focus and announcements for accessibility to ensure consistency. All the code used below is available in a [<u>nice Github repo</u>](https://github.com/rahgurung/AccessibilityVoiceOverFocusIssues/) where you can fire the app on your mobile and run all working examples. While running these code, make sure you turn over Voiceover so that you can test the behaviors. I would suggest running these on real physical device as simulators are not really trustable for testing Voiceover behaviors.

> How to reproduce this issue?

#### Announcement bug

Announcement bugs are more prevalent in an app. It is quite straightforward to reproduce. We can create one label and one button to reproduce the announcement issue. When this button is tapped, we will update the label’s text and announce `Text is updated` . Below is the code example for that:

{% codeblock lang:js %}
  class AnnouncementViewController: UIViewController {
      let label = UILabel()  
      let actionButton = UIButton()  
    
      override func viewDidLoad() {  
          super.viewDidLoad()  
          view.backgroundColor = .white  
    
          // Set up label  
          label.text = "Old Text"  
          label.textAlignment = .center  
          label.isAccessibilityElement = true  
          label.accessibilityTraits = .staticText  
    
          // Set up button  
          actionButton.setTitle("Tap Me", for: .normal)  
          actionButton.backgroundColor = .systemBlue  
          actionButton.setTitleColor(.white, for: .normal)  
          actionButton.layer.cornerRadius = 8  
          actionButton.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)  
    
          // Create a stack view for centering  
          let stackView = UIStackView(arrangedSubviews: [label, actionButton])  
          stackView.axis = .vertical  
          stackView.spacing = 5  
          stackView.alignment = .fill  
          stackView.distribution = .fill  
          stackView.translatesAutoresizingMaskIntoConstraints = false  
    
          view.addSubview(stackView)  
    
          // Center stack view  
          NSLayoutConstraint.activate([  
              stackView.centerXAnchor.constraint(equalTo: view.centerXAnchor),  
              stackView.centerYAnchor.constraint(equalTo: view.centerYAnchor),  
              stackView.widthAnchor.constraint(equalToConstant: 150)  
          ])  
    }  
    
    @objc func buttonTapped() {  
        label.text = "Updated Text"  
          
        // ❌ No announcement happens  
        UIAccessibility.post(notification: .announcement, argument: "Text is updated")  
    }  
  }
{% endcodeblock %}

When you run the code, just swipe to the button and double tap to tap it. You will notice that there is no announcement called `Text is updated`.

#### Refocus Bug

Bugs related to refocus are found in more complex view controllers. These issues are not as prevalent as announcement bugs. Mostly these bugs happens because of code written by developers itself. For example, we might be using a component which gets created with an delay, i.e. due to animation and we quickly move focus to that component in next line while it might not be available there. Below given is a code example to reproduce the issue.

{% codeblock lang:swift %}
class RefocusViewController: UIViewController {  
    let actionButton = UIButton()  
  
    override func viewDidLoad() {  
        super.viewDidLoad()
        view.backgroundColor = .white  
        actionButton.setTitle("Tap Me", for: .normal)  
        actionButton.backgroundColor = .systemBlue  
        actionButton.setTitleColor(.white, for: .normal)  
        actionButton.layer.cornerRadius = 8  
        actionButton.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)  
  
        view.addSubview(actionButton)  
        actionButton.translatesAutoresizingMaskIntoConstraints = false  
        NSLayoutConstraint.activate([  
            actionButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),  
            actionButton.centerYAnchor.constraint(equalTo: view.centerYAnchor),  
            actionButton.widthAnchor.constraint(equalToConstant: 150)  
        ])    
    }
  
    @objc private func buttonTapped() {  
        createButton()  
  
        // ❌ Focus remains stuck on the "New Button"  
        UIAccessibility.post(notification: .layoutChanged, argument: self.actionButton)  
    }  
  
    // Imagine this is some library method which you don't have access to  
    // This method creates a button with a delay and moves focus to that button  
    func createButton() {  
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.1) {  
            // Add a new focusable button (simulating a dynamic UI update)  
            let newButton = UIButton()  
            newButton.setTitle("New Button", for: .normal)  
            newButton.backgroundColor = .systemGreen  
            newButton.setTitleColor(.white, for: .normal)  
            newButton.layer.cornerRadius = 8  
            self.view.addSubview(newButton)  
  
            newButton.translatesAutoresizingMaskIntoConstraints = false  
            NSLayoutConstraint.activate([  
                newButton.centerXAnchor.constraint(equalTo: self.view.centerXAnchor),  
                newButton.topAnchor.constraint(equalTo: self.actionButton.bottomAnchor, constant: 20),  
                newButton.widthAnchor.constraint(equalToConstant: 150)  
            ])  
            UIAccessibility.post(notification: .layoutChanged, argument: newButton)  
        }  
    }  
}
{% endcodeblock %}

In the above example, we create a button which creates another button on tap. This button creation happens after a slight delay, and it moves focus to the newly created button. We do refocus on the `actionButton` itself in `buttonTapped` method but it doesn’t work.

> Why it happens?

When we post an accessibility notification (for focus change or announcements), iOS processes it based on the current accessibility hierarchy — the internal representation of UI elements that VoiceOver uses. However, if the UI is in the middle of an update (e.g., a new element is being added, a layout is changing, or a label’s text is modified), the accessibility tree may not have fully reflected these changes yet.

Since accessibility notifications are processed asynchronously, there’s a timing mismatch: the notification is sent while iOS still sees the old state of the UI. As a result:

-   For **focus changes**, VoiceOver may attempt to move focus to an element that doesn’t yet exist in the accessibility tree, causing focus to remain stuck or move unpredictably.
-   For **announcements**, if a label’s text has changed but the update hasn’t been registered in the accessibility tree, VoiceOver may ignore the announcement entirely, thinking it’s redundant.

#### Thus, mismatch between when we trigger the notification and when iOS fully registers UI updates is the root cause of the issue.

  

> How to fix it?

Since accessibility refocus or announcement issues happen because of UI element not being ready when we fire the notification. The fix is simply to introduce a delay before firing those notifications, this gives UIKit time to render the changes completely and then process the accessibility notifications.

DispatchQueue.main.asyncAfter(deadline: .now() + 0.3) {  
    UIAccessibility.post(notification: .announcement, argument: "Update completed")  
}

> Conclusion

Accessibility notifications in iOS are processed asynchronously, which can sometimes lead to announcements being ignored or focus changes not working as expected. This happens because the UI might not be fully updated when the notification is posted. By introducing slight delays and ensuring that elements are properly recognized before posting notifications, we can make VoiceOver interactions more reliable. Understanding these nuances helps create a smoother and more accessible experience for all users.
