# trigger

## trigger: (name?: string | string[]) => Promise<boolean>

### 이 메서드는 양식 또는 입력 유효성 검사를 수동으로 실행합니다. 이 방법은 종속된 유효성 검사가 있는 경우에도 유용합니다 (입력 유효성 검사가 다른 입력 값에 의존하는 경우).


### ❗ Props

|Name|Type|Description|Example|
|----|----|------------|------|
|name|`undefined`|정의되지 않음 모든 필드에 대한 유효성 검사를 트리거합니다.|`trigger()`|
||`string`|문자열 이름을 사용하여 특정 필드의 값을 유효성 검사합니다. |`trigger("yourDetails.firstName")`|
||`string[]`|문자열 배열 이름을 사용하여 여러 필드에 대한 유효성 검사를 트리거합니다.|`trigger(["yourDetails.lastName"])`|
|shouldFocus|`boolean`|오류가 발생할 때 입력란에 포커스를 맞출지 여부를 설정합니다. 이 옵션은 입력 참조가 등록되어 있을 때만 작동하며 사용자 정의 등록의 경우에는 작동하지 않습니다.|`trigger('name', { shouldFocus: true })`|

> ❗ 규칙 <br><br>
 `Isolate render optimization` 은 특정 필드의 유효성 검사를 다시 렌더링하는 데 사용되며, 이것이 배열로 공급되거나 정의되지 않은 경우에는 전체 양식 상태를 다시 렌더링합니다.

 ```tsx
 import * as React from "react";
import * as ReactDOM from "react-dom";
import { useForm } from "react-hook-form";

import "./styles.css";

type FormInputs = {
  firstName: string;
  lastName: string;
  username: string;
};

function App() {
  const {
    register,
    formState: { errors },
    trigger,
    handleSubmit,
    clearErrors
  } = useForm<FormInputs>({
    mode: "onChange"
  });

  const onSubmit = (data: FormInputs) => {
    alert(JSON.stringify(data));
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label>First name: </label>
      <input {...register("firstName", { required: true })} />
      {errors.firstName && <p>This field is Required</p>}

      <label>Last name: </label>
      <input {...register("lastName", { required: true })} />
      {errors.lastName && <p>This field is Required</p>}

      <label>Username: </label>
      <input {...register("username", { required: true })} />
      {errors.username && <p>This field is Required</p>}
      <button
        type="button"
        onClick={() => {
          clearErrors();
        }}
      >
        Clear Errors
      </button>
      <input type="submit" />
      <button
        type="button"
        onClick={() => {
          trigger();
        }}
      >
        Validate All
      </button>
      <button
        type="button"
        onClick={() => {
          trigger("firstName");
        }}
      >
        Validate First Name
      </button>
      <button
        type="button"
        onClick={() => {
          trigger(["firstName", "lastName"]);
        }}
      >
        Validate First And Last Name
      </button>
      <button
        type="button"
        onClick={async () => {
          console.log("firstName", await trigger("firstName"));
        }}
      >
        Trigger Async First Name Validation
      </button>
    </form>
  );
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);

 ```