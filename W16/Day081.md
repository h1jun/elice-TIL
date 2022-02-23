# 📖 오늘 배운 내용 - 2022.02.19

- 파이 차트 퍼센트 계산 로직 수정
- 페이지에서 발생하는 버그 및 css를 전체적으로 수정
- forEach로 코드 개선

## 📝 파이차트 퍼센트 계산 로직 수정

**🛠 기존 로직**

```jsx
const denominator = reposLanguage.length;

for (let i = 0; i < language.length; i += 1) {
  const valueCal = +((values[i] / denominator) * 100).toFixed(2);
  codeRatioArray.push({
    name: language[i],
    value: valueCal,
  });
}
codeRatioArray.sort((a, b) => b.value - a.value);
codeRatioArray = codeRatioArray.slice(0, 4);
```

- 기존 로직은 백분율 계산 후 상위 4개 언어만 보여주고 그 뒤에 언어 통계를 보여주지 않기 때문에 총합에 100%가 되지 않는 문제가 발생했다.

**🛠 로직 개선 코드**

![1](https://user-images.githubusercontent.com/81283255/155284000-23fe20b8-7089-487e-8f01-e2144b8c9395.png)

```jsx
const codeRatioArray = [];
const language = Object.keys(languageCountObj);
const values = Object.values(languageCountObj);

const denominator = reposLanguage.length;
let other = 0;

language.forEach((it, idx) => {
  if (idx >= 4) return;

  const valueCal = +((values[idx] / denominator) * 100).toFixed(2);
  other += valueCal;
  codeRatioArray.push({
    name: language[idx],
    value: valueCal,
  });
});

codeRatioArray.sort((a, b) => b.value - a.value);

if (other !== 100) {
  codeRatioArray.push({ name: "Other", value: (100 - other).toFixed(2) });
}
```

- 4개까지만 계산하고 100-other로 계산한 값을 `{ name: "Other", value: (100 - other).toFixed(2) }`을 codeRatioArray 배열 마지막에 other 언어를 넣는 로직으로 수정했다.

**🛠 로직 다시 한 번 개선**

```jsx
const languageCountObj = {};

reposLanguage.forEach((it) => {
  if (Object.prototype.hasOwnProperty.call(languageCountObj, it.language)) {
    languageCountObj[it.language] += 1;
  } else {
    languageCountObj[it.language] = 1;
  }
});

const language = Object.keys(languageCountObj);
const values = Object.values(languageCountObj);

let repoLanguageArray = [];
language.forEach((it, idx) => {
  repoLanguageArray.push({
    name: language[idx],
    value: +values[idx],
  });
});

repoLanguageArray = repoLanguageArray
  .sort((a, b) => b.value - a.value)
  .slice(0, 5);

let denominator = 0;
repoLanguageArray.forEach((it) => {
  denominator += it.value;
});

const codeRatioArray = [];
repoLanguageArray.forEach((it) => {
  codeRatioArray.push({
    name: it.name,
    value: +((it.value / denominator) * 100).toFixed(2),
  });
});
```

- 변경된 로직은 상위 5개 언어 비율을 보여주고 나머지 비율은 other에 넣어줘서 보여줬는데, 아래 사진처럼 언어 비율 4위 언어인 Lasso(**5.41%**)가 Other(kotlin+loke = **8.11%**) 보다 작게 나오다 보니 로직을 수정하게 되었다.

![2](https://user-images.githubusercontent.com/81283255/155284007-a731deec-1faf-4f7a-9200-2c0a88d6c2ea.png)

- other 언어는 삭제하기로 결정했다.
- 상위 5개의 언어를 추출하고 그 안에서 비율을 계산하는 로직으로 수정했다.
- 당시 밤새고 정신이 없어서 그런지 간단한 로직 수정에도 머리가 잘 안 돌아갔다. 코드를 봐도 반복문을 많이 돌고 있어서 비효율적인 코드라고 생각한다. 리팩토링이 필요하다!!!

## 💡 오늘 깨달은 것

- 오늘은 프로젝트 마감일이다.
- 오늘은 웹폰트 적용 시 최적화 방법을 찾고 적용하려고 했지만, 배포 전 발생하는 에러들을 수정하다 보니 하루가 다 지나갔다.
- 로그인 안 했는데 페이지 주소로 들어오면 로그인 페이지로 리다이렉트 시켜줘야 한다.
  - 프론트에서 쿠키 확인해서 존재하면 페이지 이동, 없으면 로그인 페이지로 이동
- 프로젝트가 끝나도 웹폰트 적용, 테스트 코드 작성, TS로 마이그레이션 작업을 진행할 것이다.
- 배치 작업이란 예약된 작업을 수행해주는 것을 말한다.
  - 시간별로 요청을 보내서 github api에서 정보를 가져오는 작업을 수행할 수 있다.

## 📌 참고

- [객체 + forEach( )\]](https://velog.io/@dlrbwls0302/TIL-객체-forEach)

- [[React\] 렌더시 깜빡거림 해결 Trouble shooting](https://velog.io/@dev_hikun/React-렌더시-깜빡거림-해결-Trouble-shooting)

- [Introduction](https://gseok.gitbooks.io/react/content/)

- [배치(Batch) 란 ?](https://gold-jae.tistory.com/46)
