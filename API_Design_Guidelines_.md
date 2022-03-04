---
layout: default
title: 
---

# API Design Guidelines 번역 가이드

야곰 아카데미의 API Design Guidelines 공유 문서는 원문과 번역문을 병기하는 방식으로 작성합니다. 아래의 내용을 확인해주세요.

<br>

## 📮 필독사항

- **반드시 [`Wiki`](https://github.com/yagom-academy/yagom-academy.github.io/wiki) 에서 용어를 참고하여 정확하고 일관된 용어를 사용합니다.**
- `-니다.` 체를 사용합니다.
- 내가 작성(번역)한 문장을 소리내어 읽어보았을 때 숨이 차거나 부자연스럽다면 문장을 짧게 끊어봐도 좋습니다.
- 가능하면 번역체가 아닌 한국어 표현으로 번역합니다.
    
    > 🔎 **예시**   
    Swift 코드는 간결할 수 있지만 가장 적은 문자로 가능한 가장 작은 코드를 활성화하는 것은 목표가 아닙니다.    
    ⇒ Swift는 간결하게 사용할 수 있지만, 가능한 가장 적은 문자와 코드를 사용하는 것이 목적은 아닙니다.
    > 

<br>

## ✍🏻 원문과 번역문 병기하여 작업하기

원문과 번역문은 아래와 같은 형식으로 병기합니다.

### 🔎 원문 토글이 접힌 상태

<img alt="image" src="https://user-images.githubusercontent.com/73867548/156684883-25ece5ea-0cdb-4cd3-95c4-a4004dc037cc.png">


### 🔎 원문 토글을 펼친 상태

<img alt="image" src="https://user-images.githubusercontent.com/73867548/156684899-eecd9e5e-993d-4506-91a1-3732cca55f4d.png">

- 담당한 번역 파트의 원문은 토글을 눌러서 확인할 수 있습니다.
- 번역문은 반드시 원문 아래에 작성합니다.
- 번역문의 양식은 원문의 양식과 완전 일치하도록 작성합니다. 아래의 작업 팁을 참고해주세요.

<br>

## ✍🏻 작업 팁

1. 저장소를 clone합니다.
2. `docs - apiDesignGuidelines - 담당 파트의 마크다운` 으로 접근합니다.
3. 원문의 양식과 완전히 일치하도록 작성합니다. 
    - 들여쓰기와 줄바꿈 등을 원문과 완전히 일치시킵니다.
    - 토글로 작성된 `DETAIL`은 토글 형식을 살려서 그대로 작성합니다.
    - 단, 번역문 전체는 토글에 넣지 않습니다. (위의 사진을 확인해주세요.)
    - 양식을 완전 일치시키기 위해 담당 파트의 원문 마크다운을 바로 아래에 복사하여 작업하는 것을 권장합니다.

<br>

- **참고: 토글 마크다운 형식은 아래와 같습니다.**

```markdown
<details>
<summary>Special Instructions 원문보기</summary>
<div markdown="1">

원문 내용...

</div>
</details>
```
