# ๐ ์ค๋ ๋ฐฐ์ด ๋ด์ฉ - 2022.02.19

- ํ์ด ์ฐจํธ ํผ์ผํธ ๊ณ์ฐ ๋ก์ง ์์ 
- ํ์ด์ง์์ ๋ฐ์ํ๋ ๋ฒ๊ทธ ๋ฐ css๋ฅผ ์ ์ฒด์ ์ผ๋ก ์์ 
- forEach๋ก ์ฝ๋ ๊ฐ์ 

## ๐ ํ์ด์ฐจํธ ํผ์ผํธ ๊ณ์ฐ ๋ก์ง ์์ 

**๐  ๊ธฐ์กด ๋ก์ง**

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

- ๊ธฐ์กด ๋ก์ง์ ๋ฐฑ๋ถ์จ ๊ณ์ฐ ํ ์์ 4๊ฐ ์ธ์ด๋ง ๋ณด์ฌ์ฃผ๊ณ  ๊ทธ ๋ค์ ์ธ์ด ํต๊ณ๋ฅผ ๋ณด์ฌ์ฃผ์ง ์๊ธฐ ๋๋ฌธ์ ์ดํฉ์ 100%๊ฐ ๋์ง ์๋ ๋ฌธ์ ๊ฐ ๋ฐ์ํ๋ค.

**๐  ๋ก์ง ๊ฐ์  ์ฝ๋**

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

- 4๊ฐ๊น์ง๋ง ๊ณ์ฐํ๊ณ  100-other๋ก ๊ณ์ฐํ ๊ฐ์ `{ name: "Other", value: (100 - other).toFixed(2) }`์ codeRatioArray ๋ฐฐ์ด ๋ง์ง๋ง์ other ์ธ์ด๋ฅผ ๋ฃ๋ ๋ก์ง์ผ๋ก ์์ ํ๋ค.

**๐  ๋ก์ง ๋ค์ ํ ๋ฒ ๊ฐ์ **

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

- ๋ณ๊ฒฝ๋ ๋ก์ง์ ์์ 5๊ฐ ์ธ์ด ๋น์จ์ ๋ณด์ฌ์ฃผ๊ณ  ๋๋จธ์ง ๋น์จ์ other์ ๋ฃ์ด์ค์ ๋ณด์ฌ์คฌ๋๋ฐ, ์๋ ์ฌ์ง์ฒ๋ผ ์ธ์ด ๋น์จ 4์ ์ธ์ด์ธ Lasso(**5.41%**)๊ฐ Other(kotlin+loke = **8.11%**) ๋ณด๋ค ์๊ฒ ๋์ค๋ค ๋ณด๋ ๋ก์ง์ ์์ ํ๊ฒ ๋์๋ค.

![2](https://user-images.githubusercontent.com/81283255/155284007-a731deec-1faf-4f7a-9200-2c0a88d6c2ea.png)

- other ์ธ์ด๋ ์ญ์ ํ๊ธฐ๋ก ๊ฒฐ์ ํ๋ค.
- ์์ 5๊ฐ์ ์ธ์ด๋ฅผ ์ถ์ถํ๊ณ  ๊ทธ ์์์ ๋น์จ์ ๊ณ์ฐํ๋ ๋ก์ง์ผ๋ก ์์ ํ๋ค.
- ๋น์ ๋ฐค์๊ณ  ์ ์ ์ด ์์ด์ ๊ทธ๋ฐ์ง ๊ฐ๋จํ ๋ก์ง ์์ ์๋ ๋จธ๋ฆฌ๊ฐ ์ ์ ๋์๊ฐ๋ค. ์ฝ๋๋ฅผ ๋ด๋ ๋ฐ๋ณต๋ฌธ์ ๋ง์ด ๋๊ณ  ์์ด์ ๋นํจ์จ์ ์ธ ์ฝ๋๋ผ๊ณ  ์๊ฐํ๋ค. ๋ฆฌํฉํ ๋ง์ด ํ์ํ๋ค!!!

## ๐ก ์ค๋ ๊นจ๋ฌ์ ๊ฒ

- ์ค๋์ ํ๋ก์ ํธ ๋ง๊ฐ์ผ์ด๋ค.
- ์ค๋์ ์นํฐํธ ์ ์ฉ ์ ์ต์ ํ ๋ฐฉ๋ฒ์ ์ฐพ๊ณ  ์ ์ฉํ๋ ค๊ณ  ํ์ง๋ง, ๋ฐฐํฌ ์  ๋ฐ์ํ๋ ์๋ฌ๋ค์ ์์ ํ๋ค ๋ณด๋ ํ๋ฃจ๊ฐ ๋ค ์ง๋๊ฐ๋ค.
- ๋ก๊ทธ์ธ ์ ํ๋๋ฐ ํ์ด์ง ์ฃผ์๋ก ๋ค์ด์ค๋ฉด ๋ก๊ทธ์ธ ํ์ด์ง๋ก ๋ฆฌ๋ค์ด๋ ํธ ์์ผ์ค์ผ ํ๋ค.
  - ํ๋ก ํธ์์ ์ฟ ํค ํ์ธํด์ ์กด์ฌํ๋ฉด ํ์ด์ง ์ด๋, ์์ผ๋ฉด ๋ก๊ทธ์ธ ํ์ด์ง๋ก ์ด๋
- ํ๋ก์ ํธ๊ฐ ๋๋๋ ์นํฐํธ ์ ์ฉ, ํ์คํธ ์ฝ๋ ์์ฑ, TS๋ก ๋ง์ด๊ทธ๋ ์ด์ ์์์ ์งํํ  ๊ฒ์ด๋ค.
- ๋ฐฐ์น ์์์ด๋ ์์ฝ๋ ์์์ ์ํํด์ฃผ๋ ๊ฒ์ ๋งํ๋ค.
  - ์๊ฐ๋ณ๋ก ์์ฒญ์ ๋ณด๋ด์ github api์์ ์ ๋ณด๋ฅผ ๊ฐ์ ธ์ค๋ ์์์ ์ํํ  ์ ์๋ค.

## ๐ ์ฐธ๊ณ 

- [๊ฐ์ฒด + forEach( )\]](https://velog.io/@dlrbwls0302/TIL-๊ฐ์ฒด-forEach)

- [[React\] ๋ ๋์ ๊น๋นก๊ฑฐ๋ฆผ ํด๊ฒฐ Trouble shooting](https://velog.io/@dev_hikun/React-๋ ๋์-๊น๋นก๊ฑฐ๋ฆผ-ํด๊ฒฐ-Trouble-shooting)

- [Introduction](https://gseok.gitbooks.io/react/content/)

- [๋ฐฐ์น(Batch) ๋ ?](https://gold-jae.tistory.com/46)
