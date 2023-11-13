# handleSubmit

## </> handleSubmit: `((data: Object, e?: Event) => Promise<void>, (errors: Object, e?: Event) => void) => Promise<void>`

### 이 함수는 양식(폼) 유효성 검사가 성공적으로 완료된 경우 양식 데이터를 받게 됩니다.

### ❗ Props

| Name               | Type                                           | Description    |
| ------------------ | ---------------------------------------------- | -------------- |
| SubmitHandler      | `(data: Object, e?: Event) => Promise<void>`   | 성공한 콜백    |
| SubmitErrorHandler | `(errors: Object, e?: Event) => Promise<void>` | 에러 발생 콜백 |

### ❗ 규칙 <br><br>

- handleSubmit을 사용하면 양식을 비동기적으로 쉽게 제출할 수 있습니다.

```ts
handleSubmit(onSubmit)();
// 비동기적인 유효성 검사를 위해 async 함수를 전달할 수 있습니다.
handleSubmit(async (data) => await fetchAPI(data));
```

- 비활성화된 `input`은 양식 값에서 정의되지 않은`(undefined)` 값으로 나타납니다. `input`을 업데이트할 수 없게 하고 양식 값을 유지하려면 `readOnly`를 사용하거나 전체 `<fieldset />`을 비활성화`(disable)`할 수 있습니다.

- `handleSubmit` 함수는 `onSubmit` 콜백 내부에서 발생한 오류를 자동으로 처리하지 않습니다. 그러므로 비동기 요청 내에서 오류를 시도하고, 발생한 오류를 고객에게 보기 좋게 처리하기 위해 예외 처리를 권장합니다.

```ts
const onSubmit = async () => {
  // async request 에러를 발생시킬 것 입니다.
  try {
    // await fetch()
  } catch (e) {
    // 에러를 처리하세요
  }
};

<form onSubmit={handleSubmit(onSubmit)} />;
```

### Examples

- Sync

```ts
import React from "react";
import { useForm, SubmitHandler } from "react-hook-form";

type FormValues = {
  firstName: string;
  lastName: string;
  email: string;
};

export default function App() {
  const { register, handleSubmit } = useForm<FormValues>();
  const onSubmit: SubmitHandler<FormValues> = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName")} />
      <input {...register("lastName")} />
      <input type="email" {...register("email")} />

      <input type="submit" />
    </form>
  );
}
```

- Async

```ts
import React from "react";
import { useForm } from "react-hook-form";

const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

function App() {
  const { register, handleSubmit, formState: { errors }, formState } = useForm();
  const onSubmit = async data => {
    await sleep(2000);
    if (data.username === "bill") {
      alert(JSON.stringify(data));
    } else {
      alert("There is an error");
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="username">User Name</label>
      <input placeholder="Bill" {...register("username"} />

      <input type="submit" />
    </form>
  );
}
```
