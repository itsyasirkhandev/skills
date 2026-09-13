# Spec Reference Examples

This file contains a canonical, fully-worked example of a `spec.md`. When
generating a new specification, **match this level of detail**.

---

## Example: Chat History Sidebar

> Below is a complete, production-quality spec produced with the `create-spec`

---

# Chat History Sidebar Specification

## 1. Problem Statement

As users interact with the application, they create multiple conversations over time. However, there is currently no easy way to revisit or continue past chats.

This makes it difficult for users to:

* Find previous discussions
* Resume earlier conversations
* Manage multiple chat threads

Solution: This feature introduces a chat history sidebar to solve this problem.

## 2. Functional Requirements

The system should:

* Display a sidebar showing a list of past chats
* Show a short, readable title for each chat
* Automatically generate the title from the user's first message
* Allow users to click on any chat to open it
* Highlight the currently active chat
* Add a new chat to the list after the first message is sent
* Display chats in order from most recent to oldest

## 3. inputs and outputs: Chat Opening Behavior

**USER ACTION (INPUT)**
When a user clicks on a chat from the sidebar

**EXPECTED SYSTEM BEHAVIOR**

* Open the selected chat in the main chat area
* Display the full conversation
* Show messages in the order they were originally sent
* Highlight the selected chat in the sidebar

## 4. Constraints

* The sidebar should load quickly (within 1 second for typical usage)
* Titles should be short and readable (shortened with " ... " if too long)
* The sidebar should work properly on standard laptop screens
* The system should handle a reasonable number of chats smoothly

## 5. Edge Cases and Error Handling

* **No previous chats exist**
* Show message: "No chat history yet"


* **User clicks a chat that cannot be loaded**
* Show message: "This chat could not be opened"


* **Very long first message**
* Use only the first part as the title


* **Multiple chats with similar titles**
* Allowed [each chat is still unique)


* **System fails to load chats**
* Show message: "Unable to load chat history. Please try again."


* **Large number of chats**
* Sidebar should remain scrollable



## 6. Acceptance Criteria

This feature is considered complete if:

* Users can see a list of their past chats
* The correct conversation is displayed every time
* Users can click and reopen any chat
* The active chat is clearly highlighted
* New chats appear automatically after first use
* Chats are ordered from most recent to oldest
* Empty and error states are handled properly
* Long titles are displayed neatly without breaking the layout