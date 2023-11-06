# getFieldState

## </> getFieldState: (name: string, formState?: Object) => ({isDirty, isTouched, invalid, error})


### 이 메서드는 `react-hook-form` 버전 `7.25.0`에서 소개되었으며, 개별 필드 상태를 반환하는 데 사용됩니다. 이것은 중첩된 필드 상태를 안전한 방식으로 검색하려는 경우에 유용합니다.

### ❗ Props

|Name|Type|Description|
|-|-|-|
|name|`stirng`|등록된 필드 이름.|
|formState|`object`|이것은 선택적인 속성으로, `useForm`, `useFormContext` 또는 `useFormState`에서 `formState`가 읽거나 구독되지 않았을 때에만 필요합니다. 자세한 정보는 규칙을 읽어보세요.|

### ❗ Props
|Name|Type|Description|
|-|-|-|
|isDirty|`boolean`|필드가 수정되었는지 여부를 나타냅니다. 조건: `dirtyFields`를 구독합니다.|
|isTouched|`boolean`|필드가 포커스 및 블러 이벤트를 받았는지 여부를 나타냅니다. 조건: `touchedFields`를 구독합니다.|
|invalid|`boolean`|필드가 유효하지 않은지 여부를 나타냅니다. 조건: `errors`를 구독합니다.|
|error|`undefined` `FieldError`|필드 오류 객체를 나타냅니다. 조건: `errors`를 구독합니다.|


### ❗ 규칙 <br><br>
  - `name`은 등록된 필드 이름과 일치해야 합니다
  ```tsx
    getFieldState("test")
    getFieldState("test") // ✅ register input and return field state
    getFieldState("non-existent-name") // ❌ will return state as false and error as undefined
  ```
  - `getFieldState는` `formState` 업데이트를 구독함으로써 작동하며, 다음과 같은 방법으로 `formState`를 구독할 수 있습니다.
    
    - `useForm`, `useFormContext`, 또는 `useFormState를` 사용하여 구독할 수 있습니다. 이렇게 하면 `form` `state`를 구독하며 `getFieldState`의 두 번째 인수는 더 이상 필요하지 않을 것입니다. <br><br>

    ```tsx
    const {
    register,
    formState: { isDirty },
    } = useForm()
    register("test")
    getFieldState("test") // ✅
    
    ```
    ```tsx
    const { isDirty } = useFormState()
    register("test")
    getFieldState("test") // ✅
    
    ```
    ```tsx
    const {
    register,
    formState: { isDirty },
    } = useFormContext()
    register("test")
    getFieldState("test") // ✅
    
    ```
    - `form` `state` 구독이 설정되지 않은 경우, 다음 예제를 따라 두 번째 선택적 인수로 전체 `formState`를 전달할 수 있습니다: <br><br>
    ```tsx
    const { register } = useForm()
    register("test")
    const { isDirty } = getFieldState("test") // ❌ formState isDirty is not subscribed at useForm
    const { register, formState } = useForm()
    const { isDirty } = getFieldState("test", formState) // ✅ formState.isDirty subscribed
    const { formState } = useFormContext()
    const { touchedFields } = getFieldState("test", formState) // ✅ formState.touchedFields subscribed
    ```
---
 ```tsx
 import * as React from "react"
 import { useForm } from "react-hook-form"
 export default function App() {
 const {
     register,
     getFieldState,
     formState: { isDirty, isValid },
 } = useForm({
     mode: "onChange",
     defaultValues: {
     firstName: "",
     },
 })
 // you can invoke before render or within the render function
 const fieldState = getFieldState("firstName")
 return (
         <form>
             <input {...register("firstName", { required: true })} />{" "}
             <p>{getFieldState("firstName").isDirty && "dirty"}</p>{" "}
             <p>{getFieldState("firstName").isTouched && "touched"}</p>
             <button
                 type="button"
                 onClick={() => console.log(getFieldState("firstName"))}
             >
                 field state
             </button>
         </form>
     )
 }
 ```

 ### 🙋‍♂️ 첨언
 > 조금 헷갈릴 수 도 있는데 formState를 객체로 전달 할 수 있고 useForm 리턴값에서 구조분해 해서 전달 할 수 있다. 만약 그냥 객체로 리턴할 경우에는 `const fieldState = getFieldState("firstName");` 해당 코드처럼 사용하면 된다, 하지만 물론 객체값을 구조분해로 리턴안하고 해당 값을 사용 할 수 있지다. 하지만 그러면 제대로 구독된 상태가 아니라 상태가 변해도 변했다는걸 감지하지 못한다. 그리고 만약 객체값을 구조분해로 리턴하지도 않고 객체로 리턴한다면 `getFieldState("firstName", formState);` 이렇게 리턴된 객체값을 전달해줘야 해당 훅을 사용 할 수 있다.

 ```tsx
 import * as React from "react";
import { useForm } from "react-hook-form";
import Headers from "./Header";
import "./styles.css";

let renderCount = 0;

export default function App() {
  const {
    register,
    getFieldState,
    formState,
    formState: { isDirty, dirtyFields, touchedFields, isValid }
  } = useForm({
    mode: "onChange",
    defaultValues: {
      firstName: ""
    }
  });

  renderCount++;

  // you can invoke before render or within the render function
  // 위에서 formState 객체가 구조분해되어 리턴되기 때문에 해당 값을 잘 사용하며 { isDirty, dirtyFields, touchedFields, isValid } 값들의 상태를
  // 바로바로 감지할 수 있다
  const fieldState = getFieldState("firstName");

  // when formState is not subscrbeid, you can supply formState as argument
  // formState 객체로 리턴하고 있어서 해당 객체를 두번째 인자로 넣어주어야 한다.
  getFieldState("firstName", formState);

  return (
    <form>
      <Headers renderCount={renderCount} />
      <input {...register("firstName", { required: true })} />
      <p>{getFieldState("firstName").isDirty && "dirty"}</p>
      <p>{getFieldState("firstName").isTouched && "touched"}</p>
      <button
        type="button"
        onClick={() => console.log(getFieldState("firstName"))}
      >
        field state
      </button>
    </form>
  );
}
 
 ```