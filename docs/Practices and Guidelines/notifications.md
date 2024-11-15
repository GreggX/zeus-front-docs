# Notifications

## Using Global Toast Notifications

### Overview

In our project, notifications are handled through a global `ToastContext` provided by the `GlobalProvider`. This allows us to easily display notifications across the application without needing to manage individual state for each notification.

The primary method for triggering a notification is the `addToast` function, which accepts a configuration object to define the notification's appearance and behavior.

---

### How to Use `addToast`

To trigger a notification, call the `addToast` function from the `ToastContext`. You can customize the notification using the `ZeusAlertComponentProps` interface.

#### Example Usage

```typescript
import { useContext } from "react";
import { ToastContext } from "../context/GlobalProvider";

const MyComponent = () => {
  const { addToast } = useContext(ToastContext);

  const handleSuccessNotification = () => {
    addToast({
      type: "success",
      visible: true,
      title: "Operation Successful",
      subtitle: "Your changes have been saved successfully.",
      animation: "FadeIn",
      height: 50,
      width: 400,
    });
  };

  const handleErrorNotification = () => {
    addToast({
      type: "error",
      visible: true,
      title: "Operation Failed",
      subtitle: "Something went wrong. Please try again later.",
      animation: "ScaleUp",
      height: 50,
      width: 400,
    });
  };

  return (
    <div>
      <button onClick={handleSuccessNotification}>Show Success</button>
      <button onClick={handleErrorNotification}>Show Error</button>
    </div>
  );
};

export default MyComponent;
```

---

### `ZeusAlertComponentProps`: Notification Configuration

The `addToast` function accepts an object adhering to the following structure:

```typescript
export interface ZeusAlertComponentProps {
  title?: string; // The main title of the notification
  subtitle?: string; // The additional text providing details
  animation?: "ScaleUp" | "FadeIn"; // Animation style for showing the notification
  height?: number; // Height of the notification in pixels
  width?: number; // Width of the notification in pixels
  type: "success" | "error" | "warning" | "info"; // The type of notification
  visible: boolean; // Whether the notification is visible
  onCloseAlert?: () => void; // Callback triggered when the notification is dismissed
}
```

### Example Configuration

#### Success Notification

```typescript
addToast({
  type: "success",
  visible: true,
  title: "Operation Completed",
  subtitle: "Everything worked perfectly!",
  animation: "FadeIn",
  height: 50,
  width: 300,
});
```

#### Error Notification

```typescript
addToast({
  type: "error",
  visible: true,
  title: "Error Occurred",
  subtitle: "Please check your input and try again.",
  animation: "ScaleUp",
  height: 50,
  width: 400,
});
```

---

### Best Practices

1. **Centralized Notifications**:
   - Leverage the `ToastContext` to manage all notifications globally. This ensures consistency in behavior and styling.

2. **Customizing Animations**:
   - Choose between `"ScaleUp"` and `"FadeIn"` animations based on the type of feedback you're providing. For example:
     - Use `"FadeIn"` for success or info messages.
     - Use `"ScaleUp"` for errors or warnings to draw more attention.

3. **Keep Notifications Concise**:
   - Avoid overly detailed subtitles. Provide a clear and concise message that informs the user of the key event.

4. **Handle Visibility**:
   - The `visible` property should always be set to `true` when displaying the notification. Optionally, implement an `onCloseAlert` callback for dismiss actions.

---

### Additional Example: Triggering Notifications Based on API Status

Here’s an example of triggering notifications based on the status of an API call:

```typescript
const handleApiCall = async () => {
  try {
    const response = await someApiCall();
    addToast({
      type: "success",
      visible: true,
      title: "Data Saved",
      subtitle: "The data has been successfully saved to the server.",
      animation: "FadeIn",
      height: 50,
      width: 420,
    });
  } catch (error) {
    addToast({
      type: "error",
      visible: true,
      title: "Error Saving Data",
      subtitle: error.message || "Please try again later.",
      animation: "ScaleUp",
      height: 50,
      width: 420,
    });
  }
};
```

---

### Summary

- **Global Context**: Notifications are handled through the `ToastContext` from the `GlobalProvider`.
- **Customization**: Use the `ZeusAlertComponentProps` interface to define the appearance and behavior of notifications.
- **Ease of Use**: The `addToast` function makes it simple to display notifications with minimal code.

With this approach, notifications in your project are consistent, customizable, and easy to manage.
