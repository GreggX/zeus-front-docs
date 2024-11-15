# Forms

## Introduction

In this project, we highly recommend using `Yup` schemas for validation combined with the `useForm` hook. This approach ensures consistency, simplifies form handling, and reduces boilerplate code.

The `useForm` hook streamlines form management by providing a simple API for handling values, errors, and validation status. Here’s a breakdown of how to use it effectively.

---

### Setting Up a Form with `useForm`

#### 1. **Creating a Yup Schema**

Define a Yup schema to validate your form fields. Each field’s validation rules should align with the form's requirements.

```typescript
import * as Yup from "yup";

const Schema = Yup.object().shape({
  name: Yup.string()
    .max(50, "Ha excedido el máximo número de caracteres")
    .matches(/^[a-zA-Z\s]*$/, "Por favor ingrese un nombre válido")
    .required("Este campo es requerido"),
  phone: Yup.string()
    .length(10, "El número debe tener 10 dígitos")
    .matches(/^\d+$/, "Solo se permiten números")
    .required("Este campo es requerido"),
});
```

#### 2. **Initializing the `useForm` Hook**

```typescript
const { form, errors, handleChange, setFormValue, isValid } = useForm({
  schema: Schema,
  initialValues: {
    name: "",
    phone: "",
  },
});
```

- `form`: Contains the current values of the form fields.
- `errors`: Contains validation errors for each field.
- `handleChange`: Automatically validates fields when the user types.
- `setFormValue`: Allows you to programmatically set and validate a field's value.
- `isValid`: Indicates whether the form is valid based on the current values.

---

### Automatic Validation Based on `name` Attribute

When using `useForm`, the `name` attribute of each input field must match the corresponding key in the Yup schema. Validation occurs automatically as the user types.

#### Example: Basic Field Validation

```tsx
<ZeusTextInput
  counterValidation={{
    maxLength: 50,
    visible: false,
  }}
  onChange={handleChange}
  maxLength={50}
  width="100%"
  name="name" // Matches the key in the Yup schema
  value={form.name}
  placeholder="Nombre"
/>
{errors.name && <p className="error">{errors.name.message}</p>}
```

In this example:

- The field is validated against the `name` key in the Yup schema.
- Errors are displayed conditionally.

---

### Custom Formatting for Input Fields

For inputs that require custom formatting or pre-processing (e.g., formatting phone numbers), use the `setFormValue` function.

#### Example: Custom Formatting for a Phone Number

```tsx
<ZeusTextInput
  counterValidation={{
    maxLength: 10,
    visible: false,
  }}
  onChange={(e) => {
    const number = strToNumber(e.currentTarget.value); // Custom formatting logic
    const phone = cleanZero(number); // Remove leading zeroes
    setFormValue("phone", phone); // Programmatically set the value
  }}
  maxLength={10}
  width="100%"
  name="phone" // Matches the key in the Yup schema
  value={form.phone}
  placeholder="Teléfono celular"
/>
{errors.phone && <p className="error">{errors.phone.message}</p>}
```

In this example:

- Input values are formatted before being validated.
- Errors are still handled based on the Yup schema.

---

### Advanced Example: Full Form Implementation

Here’s a complete example combining multiple fields and a submit button:

```tsx
import React from "react";
import * as Yup from "yup";
import { ZeusTextInput, ZeusButton } from "zeus-front-design-system";
import useForm from "./hooks/useForm";

const Schema = Yup.object().shape({
  name: Yup.string()
    .max(50, "Ha excedido el máximo número de caracteres")
    .matches(/^[a-zA-Z\s]*$/, "Por favor ingrese un nombre válido")
    .required("Este campo es requerido"),
  phone: Yup.string()
    .length(10, "El número debe tener 10 dígitos")
    .matches(/^\d+$/, "Solo se permiten números")
    .required("Este campo es requerido"),
});

const MyForm = () => {
  const {
    form,
    errors,
    handleChange,
    setFormValue,
    isValid,
    hasFormChange,
  } = useForm({
    schema: Schema,
    initialValues: {
      name: "",
      phone: "",
    },
  });

  const handleSubmit = () => {
    if (!isValid) {
      alert("Por favor, corrige los errores.");
      return;
    }

    console.log("Form submitted:", form);
  };

  return (
    <form>
      <ZeusTextInput
        counterValidation={{
          maxLength: 50,
          visible: false,
        }}
        onChange={handleChange}
        maxLength={50}
        width="100%"
        name="name"
        value={form.name}
        placeholder="Nombre"
      />
      {errors.name && <p className="error">{errors.name.message}</p>}

      <ZeusTextInput
        counterValidation={{
          maxLength: 10,
          visible: false,
        }}
        onChange={(e) => {
          const number = strToNumber(e.currentTarget.value);
          const phone = cleanZero(number);
          setFormValue("phone", phone);
        }}
        maxLength={10}
        width="100%"
        name="phone"
        value={form.phone}
        placeholder="Teléfono celular"
      />
      {errors.phone && <p className="error">{errors.phone.message}</p>}

      <ZeusButton
        type="submit"
        onClick={(e) => {
          e.preventDefault();
          handleSubmit();
        }}
        disabled={!hasFormChange || !isValid}
      >
        Submit
      </ZeusButton>
    </form>
  );
};

export default MyForm;
```

---

### Key Takeaways

1. **Validation Tied to Schema**:
   - The `name` attribute of inputs must match the keys in the Yup schema for automatic validation.

2. **Custom Logic**:
   - Use `setFormValue` to handle special cases where input needs pre-processing before validation.

3. **Error Handling**:
   - Display error messages using `errors` from the `useForm` hook.

4. **Form State**:
   - Use `isValid` to enable or disable the submit button.
   - Use `hasFormChange` to detect unsaved changes.

This structure ensures clean and maintainable form handling throughout your project.
