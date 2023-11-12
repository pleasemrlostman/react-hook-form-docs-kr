# useWatch

## **useWatch: ({ control?: Control, name?: string, defaultValue?: unknown, disabled?: boolean }) => object**

`useWatch`와 비슷하게 동작하지만 `useWatch`와 달리, 해당 FormContext에서 분리하여 상위 custom hook 단에서의 랜더링을 막아준다. 이를 통해 더 좋은 성능을 낼 수 있다.

### ❗ Props

- 다음 표는 `useWatch`에 대한 파라미터에 관한 정보를 포함하고 있습니다.

| Name | Type | Description |
| name | string, string[], undefined | `useWatch`를 사용할 필드의 이름을 입력합니다. (배열 또는 문자열) |
| control | Object | useForm에 제공해주는 control 은 formContext를 사용할 경우에 control에 methods를 넣어 사용한다. |
| defaultValue | unknown | useWatch가 첫 랜더시 가지는 값을 넣어준다. |
| disabled | boolean = false | useWatch에 걸린 구독 상태를 해제한다. 기본값은 false |
| exact | boolean = false | 정확하게 일치하는 이름을 가진 register만 useWatch를 사용하도록 하는 옵션값. |

### ❗ **Return**

| Name                               | Type                    | Description |
| ---------------------------------- | ----------------------- | ----------- |
| useWatch({ name: 'inputName' })    | unknown                 |             |
| useWatch({ name: ['inputName1'] }) | unknown[]               |             |
| useWatch()                         | {[key:string]: unknown} |             |

### ❗ Rules

- 첫 랜더링 시, useWatch에 걸려 있는 defaultValue를 참조하여 값을 설정합니다. useWatch에 defaultValue가 없는 경우, 상위 formContext에 걸려있는 defaultValue를 가져와 값을 설정합니다.

- useWatch와 watch의 차이점은 root level(최상단)에 걸려있는지, 아니면 custom hook level(혹은 컴포넌트단) 에서 업데이트가 이뤄지는가에 달려 있습니다.

- 가장 중요하게 생각해야 할 부분은, useWatch는 실행 순서에 영향을 받는다는 것 입니다. 만약, useWatch가 form state의 업데이트 이전에 선언되어 있다면, useWatch는 update를 인식하지 않습니다.

```jsx
setValue("test", "data");
useWatch({ name: "test" }); // ❌ 구독 상태 이전에 값을 update했기 때문에, 이는 useWatch가 감지하지 않습니다.
useWatch({ name: "example" }); // ✅ 해당 상태는 감지 할 수 있습니다.
setValue("example", "data");
```

### ❗ Example

```tsx
import React from "react";
import { useForm, useWatch } from "react-hook-form";

interface FormInputs {
  firstName: string;
  lastName: string;
}

function FirstNameWatched({ control }: { control: Control<FormInputs> }) {
  const firstName = useWatch({
    control,
    name: "firstName", // name이 없는 경우에는 모든 form을 watch합니다. Array<string> 형태로 입력 시, 배열에 들어가 있는 form을 구독합니다.
    defaultValue: "default", // rendering 할 기본값입니다.
  });

  return <p>Watch: {firstName}</p>;
}

function App() {
  const { register, control, handleSubmit } = useForm<FormInputs>();

  const onSubmit = (data: FormInputs) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label>First Name:</label>
      <input {...register("firstName")} />
      <input {...register("lastName")} />
      <input type="submit" />

      <FirstNameWatched control={control} />
    </form>
  );
}
```

### 🙋‍♂️ 첨언

> `useWatch`는 상위 root level 에서 가져오는 `watch()`와 달리, 컴포넌트나 커스텀 훅 레벨에서 값의 변경을 감지하고 이를 `DOM`에 업데이트 하기 때문에, 상위 컴포넌트가 바뀌면 하위 `children`이 모두 바뀌는 불필요한 상황을 야기하지 않습니다.
