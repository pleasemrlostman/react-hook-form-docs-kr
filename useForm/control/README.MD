# control

## </> control: Object

이 객체는 React Hook Form에 구성 요소를 등록하는 메서드를 포함하고 있습니다.

> ❗ 규칙<br><BR>
중요: 이 객체 내의 속성에 직접 접근하지 마십시오. 이는 내부적인 사용을 위한 것입니다.

```tsx
import React from "react"
import { useForm, Controller } from "react-hook-form"
import { TextField } from "@material-ui/core"

type FormInputs = {
  firstName: string
}

function App() {
  const { control, handleSubmit } = useForm<FormInputs>()
  const onSubmit = (data: FormInputs) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        as={TextField}
        name="firstName"
        control={control}
        defaultValue=""
      />

      <input type="submit" />
    </form>
  )
}
```

### 🙋‍♂️ 첨언
> `control` 객체는 주로 useForm의 정보를 담아 useForm과 특정 hook을 연결시켜주는 역할을 한다.<br>
예를들어 `useFieldArray` 같은 경우도 `useForm`의 `control`을 전달하여 `useForm`에 `useFrieldArray` 훅의 정보를 연결해주고 (물론 연결이란 표현은 본인이 임의로 작성한 표현이다) `useFormState` 역시 `useForm`의 `control`을 인자로 받아 연결된 `useForm`의 `formState`를 사용할 수 있다.