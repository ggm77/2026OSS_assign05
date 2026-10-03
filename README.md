# Assignment 05 - Simple CRUD Service

## Deployment

- Vercel URL: https://2026ossassign05-green.vercel.app/

| 페이지 | 설명 |
|---|---|
| `index.html` | 두 페이지로 이동하는 링크 모음 |
| `js_dynamic.html` | JavaScript DOM 연습 (입력 → 목록 추가 / 삭제) |
| `crud.html` | 메모 관리 CRUD Service |

## Key Learning

1. HTML Form 입력값을 JavaScript Array에 저장하고, `render()`로 Array의 내용을 DOM에 다시 그리는 흐름을 이해했다.
2. Create / Update / Delete 후에는 화면의 요소만 바꾸는 것이 아니라 Array를 먼저 바꾸고 `render()`를 다시 호출해야 한다는 것을 배웠다.
3. Validation을 통해 잘못된 입력을 막고, `alert()`와 `focus()`로 사용자에게 알려주는 방법을 배웠다.

## CRUD Service

### 주제

메모 관리

### 데이터 Field

| Field | 설명 |
|---|---|
| id | 메모 번호 (`dataIndex`로 새로 발급) |
| title | 제목 |
| content | 본문 |
| category | 카테고리 (TODO, BUY, SCHEDULE 등) |
| createdAt | 생성일 (`YYYY-MM-DD`) |

### 기능 구현 방법

- **Create**: 입력값을 검사한 뒤 `data.push()`로 Array에 추가하고, 입력창을 비운 뒤 `render()`를 호출한다.
- **Read**: `render()`가 `data.forEach()`로 항목마다 `li`를 만들어 `#list`에 붙인다. 시작할 때와 추가 / 수정 / 삭제 후에 호출한다.
- **Update**: [수정] 버튼을 누르면 해당 항목 값을 Form에 채우고 `editingId`에 id를 저장한다. 저장할 때 `editingId`가 있으면 `data.find()`로 항목을 찾아 값을 바꾸고 `render()`를 호출한다.
- **Delete**: `confirm()`으로 확인한 뒤 `data.filter()`로 해당 id를 뺀 새 Array를 만들고 `render()`를 호출한다.

### Validation

Create와 Update가 같은 `addItem()` 함수를 사용하기 때문에 두 기능 모두 같은 검사를 거친다.

- 필수 입력값 확인 (제목, 본문, 카테고리, 생성일)
- 문자열 길이 확인 (제목 20자 이하)
- 날짜 확인 (오늘 이후 날짜 불가)

## JavaScript

| 기능 | 사용한 곳 |
|---|---|
| `querySelector()` | input, button, list 요소 가져오기 |
| `addEventListener()` | 페이지 load, 추가 / 수정 / 삭제 버튼 click 처리 |
| `createElement()` | 목록의 `li`, `span`, `button` 만들기 |
| `appendChild()` | 만든 요소를 `li`와 `#list`에 붙이기 |
| `replaceChildren()` | `render()`에서 기존 목록을 비우기 |
| `push()` | Create에서 Array에 추가 |
| `find()` | Update에서 수정할 항목 찾기 |
| `filter()` | Delete에서 해당 항목을 뺀 Array 만들기 |
| `forEach()` | `render()`에서 Array의 모든 항목 그리기 |
| `trim()` | 공백만 입력한 경우 걸러내기 |
| `confirm()` / `alert()` | 삭제 확인 / Validation 메시지 |
| `focus()` | Validation 실패 시 해당 입력창으로 이동 |

## AI / Search Usage

- Tool: Claude (Claude Code)

| 사용 목적 | 실제 적용 | 새롭게 이해한 내용 |
|---|---|---|
| `p` 태그에서 연속 공백이 보이지 않는 문제 확인 | 목록 헤더 `p`에 `white-space: pre` 적용 | HTML은 연속된 공백을 하나로 합치며, `white-space: pre`나 `&nbsp;`로 공백을 유지할 수 있다. |
| `createdAt` 검사 방법 확인 | `date` input의 `value`가 비어 있는지 검사하고, 미래 날짜는 막음 | `type="date"`의 `value`는 `YYYY-MM-DD` 형식이거나 빈 문자열이다. |
| 목록 요소를 지우는 방법 확인 | `render()`에서 `list.replaceChildren()` 사용 | 비우고 다시 그리면 같은 항목이 중복으로 쌓이지 않는다. |
| Update / Delete 구현 방법 질문 | `editingId`, `find()`, `filter()`를 사용해 직접 구현 | 수정 중인 항목을 삭제하면 `editingId`도 초기화해야 한다. |
| README 작성 | AI가 README 초안을 작성하고, 내용을 확인하며 수정함 | 과제 설명에 있는 README 항목(Deployment, Key Learning, AI Usage 등)의 구성을 확인했다. |

## Problem & Solution

| 문제 | 해결 |
|---|---|
| `p` 태그에 공백을 여러 개 넣어도 한 칸만 보임 | `white-space: pre`를 적용했다. |
| `<form>` 안의 버튼을 누르면 페이지가 새로고침됨 | 버튼에 `type="button"`을 지정했다. |
| 이벤트에 `update`를 바로 넘기면 항목이 아니라 click 이벤트가 전달됨 | `() => update(item)`처럼 화살표 함수로 감쌌다. |
| 날짜 형식이 기존 데이터(`2026.09.25`)와 `date` input 값(`2026-09-25`)이 달라 섞임 | 기존 데이터를 `YYYY-MM-DD` 형식으로 통일했다. |
| Validation 실패 후 `focus()`가 동작하지 않는 것처럼 보임 | 아직 원인을 확정하지 못했다. `alert()`가 닫힌 뒤 브라우저가 포커스를 버튼으로 되돌리는 것으로 추정하고 있다. |

## Reflection

- `addItem()` 하나로 추가와 수정을 모두 처리하다 보니 `editingId`로 모드를 구분해야 해서 구조가 복잡해졌다. 추가와 수정 함수를 나누면 어떻게 달라질지 궁금하다.
