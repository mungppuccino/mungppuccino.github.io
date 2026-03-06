---
# Header
title: "고급객체지향프로그래밍 DecimalFormat"
excerpt: "DecimalFormat 설명 글"
name: mungppuccino
writer: mungppuccino
categories: [고급객체지향프로그래밍] # [메인 카테고리, 서브 카테고리]
tags:
  - [java, DecimalFormat]

toc: true
toc_sticky: true

date: 2026-03-06
last_modified_at: 2026-03-06

# --- 아래 부터 content
---
## DecimalFormat 개념
DecimalFormat는 자바에서 숫자를 원하는 형식의 문자열로 바꿀 때 사용하는 클래스다.  
- **기호: 0** | 의미: 10진수 한 자리(필수) | 특징: 값이 없으면 0으로 채움
- **기호: #** | 의미: 10진수 한 자리(선택) | 특징: 값이 없으면 공백으로 둠
- **기호: .** | 의미: 소수점 | 특징: 소수점 구분자
- **기호: ,** | 의미: 단위 구분자 | 특징: 천 단위 쉼표 사용

  ```java
    // # vs 0 차이
    double num = 123.4;
    DecimalFormat df1 = new DecimalFormat("###.###"); 
    System.out.println(df1.format(num)); // 출력: 123.4 (뒤에 빈 자리는 무시)

    DecimalFormat df2 = new DecimalFormat("000.000"); 
    System.out.println(df2.format(num)); // 출력: 123.400 (빈 자리를 0으로 채움)
    // System.out.printf("%s",df2.format(num));
    // 이때 상단에 import java.text.DecimalFormat; 해야 한다.
  ```

## DecimalForma 이해
```java
   DecimalFormat df = new DecimalFormat("#,###");
```
딱 한 번만 쉼표를 찍으라는 뜻이 아니다. 오른쪽 끝에서부터 3자리마다 쉼표를 찍으라는 의미다.  
#,### 패턴에서 자바가 가장 중요하게 보는 건 쉼표가 뒤에서부터 몇 번째 자리에 찍혀 있느냐다. #,###나 ###,###이나 똑같이 뒤에서 3번째에 쉼표가 찍히는 규칙으로만 해석된다.  

#,### 와 #,##0의 차이점  
숫자가 1234일 때 #,###은 1,234을 의미하고 #,##0은 1,234을 의미한다.  
숫자가 0일 때 #,###은 빈 문자열을 의미하고 #,##0은 0을 의미한다.

1..앞에 #,.뒤에 0을 쓸 때(자릿수 고정)  
패턴:#.00(소수점 둘째 자리까지 무조건 표시)  
12.5 → 12.50  
12 → 12.00

2..앞에 #,.뒤에 #을 쓸 때(있을 때만 표시)  
패턴: #.## (소수점 둘째 자리까지 있을 때만 표시)  
12.5 → 12.5  
12 → 12 (소수점 자체가 사라짐)  
문제점: 0.5 같은 숫자를 출력하면 .5가 나온다. 이를 해결하려면 0.###패턴을 사용해야 한다.

3..앞에 0, .뒤에 0 쓸 때  
패턴: 0.000 (정수 한 자리, 소수 세자리 무조건 채우기)  
12.5 → 12.500  
12 → 12.000

4..앞에 0, .뒤에 # 쓸 때  
패턴: 0.### (정수 한 자리 무조건 채우기)  
12.5 → 12.5  
12 → 12

DecimalFormat은 정해진 자릿수보다 더 많은 소수점이 들어오면 반올림을 해서 보여준다.  
패턴: #.## (둘째 자리까지 표시)  
입력: 3.145  
출력: 3.15 (세 번째 자리인 5에서 반올림됨)



## 예제 문제
```java
package proj0306_1;
import java.text.DecimalFormat;
public class printInfo {
	public static void main(String [] args) {
		DecimalFormat df = new DecimalFormat("#,###");
		String movieName="왕과 사는 남자";
		String actor [] = {"박지훈","유해진"};
		int price = 14000;
		int qty = 2;
		int total = price * qty;
		System.out.printf("영화 이름: %s\n",movieName);
		System.out.printf("영화 배우: ");
		int i;
		for(i=0; i<actor.length; i++)
		{
			System.out.printf("%s", actor[i]);
			if(i<(actor.length-1)) {
				System.out.printf(",");
			}
		}
		System.out.println();
		System.out.printf("영화 가격: %s\n",df.format(price));
		System.out.printf("총 수량: %d\n", qty);
		System.out.printf("총 가격: %s\n", df.format(total));
	}
}
```

    