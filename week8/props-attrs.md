# 5. props와 attrs

## 강의 요약 및 노트

- props는 활성화 여부를 표현하거나 특정 스타일을 잡아주고 싶을 때 유용하게 사용 가능
- 위와 아래는 동일하게 작동함(내부 확장인 css의 도움을 받을 수 있음-> 일반텍스트를 확장 표기해줌)

```typescript
const Button = styled.button<ButtonProps>`
    background: ${(props) => (props.active ? '#00F' : '#FFF')};
    color: ${(props) => (props.active ? '#FFF' : '#000')};
    border: 1px soid #888;
`;
```

```typescript
const Button = styled.button<ButtonProps>`
    background: '#FFF';
    color: '#000';
    border: 1px soid #888;

  ${(props) => props.active && css`
    background: #00F;
    color: #FFF;
    `}
`;
```

- 버튼이나 input 등 속성을 넣어주어야 함. 기본 속성을 매번 반복해서 넣는 것이 아니라 styled-components로 넣을 수 있음

```typescript
const Button = styled.button.attrs({
    type: 'button',
})<ButtonProps>`
    // 블라블라..
`;
```

- 강의 예제에서 나온 부분, 특히 props와 타입 잡는 법 예시 통해 공부하기

```typescript
import styled, { css } from 'styled-components';

import { useBoolean } from 'usehooks-ts';

type ButtonProps = {
  type?: 'button' | 'submit' | 'reset';
  active?: boolean;
}

const Button = styled.button.attrs<ButtonProps>((props) => ({
  type: props.type ?? 'button',
}))<ButtonProps>`
    background: '#FFF';
    color: '#000';
    border: 1px soid #888;

  ${(props) => props.active && css`
    background: #00F;
    color: #FFF;
`}
`;

export default function Switch() {
  const { value: active, toggle } = useBoolean(false);

  return (
    <Button type="submit" onClick={toggle} active={active}>
      On/Off
    </Button>
  );
}
```

<hr />

## 키워드 정리

### props

</br>

### attrs
