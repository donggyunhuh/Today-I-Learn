## Thymeleaf에서 if문은 태그 안에 [th:if]를 사용하여 처리.
```html
<div>
    <span th:if="${info.gender == 'F'}">여자</span>
    <span th:if="${info.gender == 'M'}">남자</span>
</div>
```


### if-else문의 경우는 [th:if]와 [th:unless]를 사용

``` html
<div th:if="${info.useYn == 'Y'}">사용</div>
<div th:unless="${info.useYn == 'Y'}">사용안함</div>
```
또한, th:block 블록을 사용하여 div 영역 자체에 조건을 줄 수 있다. 

``` html
<th:block th:if="${info.useYn == 'Y'}">
	<div>가능 합니다.</div>
<th:block>

<th:block th:if="${info.useYn == 'N'}">
<div>불가능 합니다.</div>
</th:block>
```
출처: https://ssd0908.tistory.com/entry/thymeleaf-if-else-조건문-사용방법 [에스제이:티스토리]