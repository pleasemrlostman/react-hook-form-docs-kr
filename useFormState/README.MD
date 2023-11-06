# useFormState

## **useFormState: ({ control: Control }) => FormState**

이 `useFormState`는 각 폼 상태를 구독하고 `useFormState` 훅 수준에서 렌더링을 분리 할 수 있게 해줍니다. 이 훅은 폼 상태 **구독** 관련하여 각각의 범위를 가지고 있으므로 다른 `useFormState`와 `useForm`에 영향을 미치지 않습니다. `useFormState`을 사용하면 크고 복잡한 `useFormState` 응용 프로그램에 대한 재 렌더링 영향을 줄일 수 있습니다.

### ❗ Props

- 다음 표는 `useFormState`에 대한 파라미터에 관한 정보를 포함하고 있습니다.

| Name     | Type            | Description                                                                                                                     |
| -------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| control  | object          | useForm에서 제공된 control 객체입니다. FormContext를 사용하는 경우 이 인수는 선택 사항입니다.                                   |
| name     | string          | string[]                                                                                                                        | 하나의 입력 이름, 여러 개의 입력 이름 배열 또는 모든 입력의 formState 업데이트를 구독하기 위한 이름을 제공합니다. |
| disabled | boolean = false | 구독을 비활성화하는 옵션입니다. 이 값이 true이면 구독이 비활성화됩니다.                                                         |
| exact    | boolean = false | 이 프롭은 입력 이름 구독에 대한 정확한 일치를 활성화합니다. 이 값이 true이면 입력 이름을 정확히 일치시켜야 구독이 활성화됩니다. |

### ❗ **Return**

| Name               | Type    | Description                                                                                                                              |
| ------------------ | ------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| isDirty            | boolean | <center>[링크](https://github.com/pleasemrlostman/react-hook-form-docs-kr/blob/main/useFormState/Return/isDirty-description.MD)</center> |
| dirtyFields        | object  |                                                                                                                                          |
| touchedFields      | object  |                                                                                                                                          |
| defaultValues      | object  |                                                                                                                                          |
| isSubmitted        | boolean |                                                                                                                                          |
| isSubmitSuccessful | boolean |                                                                                                                                          |
| isSubmitting       | boolean |                                                                                                                                          |
| isLoading          | boolean |                                                                                                                                          |
| submitCount        | number  |                                                                                                                                          |
| isValid            | boolean |                                                                                                                                          |
| isValidating       | boolean |                                                                                                                                          |
| errors             | object  |                                                                                                                                          |

### ❗ Rules

- 반환된 formState는 특정 상태가 구독되지 않으면 렌더링 성능을 향상시키고 추가 계산을 건너뛰기 위해 **Proxy**로 래핑되어 있으므로, 구독이 활성화되도록 렌더링하기 전에 해당 상태를 해체하거나 읽어야 합니다.

```jsx
const { isDirty } = useFormState(); // ✅
const formState = useFormState(); // ❌ 반드시 구조분해해서 사용해주세요
```

### ❗ Example

```jsx
import * as React from "react";
import { useForm, useFormState } from "react-hook-form";

function Child({ control }) {
  const { dirtyFields } = useFormState({
    control,
  });

  return dirtyFields.firstName ? <p>Field is dirty.</p> : null;
}

export default function App() {
  const { register, handleSubmit, control } = useForm({
    defaultValues: {
      firstName: "firstName",
    },
  });
  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName")} placeholder="First Name" />
      <Child control={control} />

      <input type="submit" />
    </form>
  );
}
```

### 🙋‍♂️ 첨언

> `useFormState`는 필요한 각각의 프로퍼티를 (ex `isDirty`, `dirtyFields`) `props`로 전달해주지 않고 `control` 리턴값 하나를 `props`로 전달받아 `useForm`의 리턴값인 `formState`를 구독할 수 있는 장점이 있다.<br>
> `useFormContext` 와는 완전히 다른 `hook`이다 `useFormContext`는 `useForm`의 리턴값인 `register`나 `setValue` 같은 값들을 사용할 수 있게 도와주는 hook이고 `useFormState`는 `useForm`의 리턴값인 `formState`를 사용할 수 있게 해주는 훅이다. 즉 `FormProvider`를 이용해 `formState`를 사용할 수 있다고 생각하면 된다.
