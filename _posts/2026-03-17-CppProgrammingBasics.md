---
# Header
title: "C++ 프로그래밍의 기본"
excerpt: "C++ 프로그래밍의 기본 설명 글"
name: mungppuccino
writer: mungppuccino
categories: [C++] # [메인 카테고리, 서브 카테고리]
tags:
  - [C++, Programming, string, char]

toc: true
toc_sticky: true

date: 2026-03-17
last_modified_at: 2026-03-17

# --- 아래 부터 content
---
# C++에서 문자열 표현
1. 널문자('\0')로 끝나는 char 배열   
2. <string> 헤더 포함 필요 - string 클래스 이용

```C++
char str1[3]={'h','i','\0'};
std::string str2 = "hi"; //include <string> 포함
```

# char 배열과 문자열의 getline 차이점
- **구분** | cin.getline(name, 10) | getline(cin, name)
- **대상 자료형** | char name[10] (배열) | std::string name (객체)
- **함수 성격** | cin 객체의 멤버 함수 | 전역 함수 (std 네임스페이스)
- **특징** | 최대 크기를 미리 지정해야 함 | 문자열 길이에 따라 자동으로 메모리 조절
- **헤더** | <iostream> | <string>

char 배열에서 최대 size-1개의 문자를 입력받는다. - 왜냐하면 마지막 문자에 '\0'가 있다. - 자동으로 붙는다.    
size-1개의 문자가 입력되지 않아도 delimitChar를 만나면 입력 중단 - string도 마찬가지

```C++
char address[100];
cin.getline(address, 100, '\0') //최대 99개의 문자를 읽어 address 배열에 저장. 도중에 <Enter> 키를 만나면 입력 중단.
```

주의점: 두 함수 모두 cin >> 뒤에 바로 사용하면, 버퍼에 남아있는 줄바꿈 문자(\n)를 읽고 바로 종료될 수 있다.

```C++
int age;
cin >> age;
cin.ignore(); // 버퍼에 남은 엔터 제거

string name;
getline(cin, name); // 이제 정상적으로 입력을 기다린다.
```

# 함수의 매개변수와 오버로딩

```C++
#include <iostream>
#include <string>
int length(int n);
int length(std::string str);
int main(void) {
	// 문자열
	std::string str = "hello";
	std::cout << length(12345) << std::endl;
	std::cout << length(str) << std::endl;
	return 0;
}
int length(int n) {
	int cnt = 0;
	while (n > 0) {
		n /= 10;
		cnt++;
	}
	return cnt;
}
int length(std::string str) {
	return str.length();
	//문자열 클래스는 str.length() / str.size() 자기자신 안에 있는 함수 쓰면 된다.
}
```

문자열 객체.c_str() - C++의 string 객체를 C언어에서 쓰던 char* (문자열 배열) 형태로 바꿔준다.   
특정 라이브러리들은 오직 char 배열(char*)만 받는게 있다.   
1. 널 문자(\0) 자동 포함: 맨 마지막에 \0이 붙은 문자열의 주소값을 반환   
2. 읽기 전용: 직접 가서 글자를 수정할 수 없음   
3. 유효 기간: str 객체가 수정되거나 메모리에서 사라지면 c_str()로 얻은 주소값도 쓸모가 없어진다.

```C++
#include <iostream>
#include <string>
int length(int n);
int length(char* str);
int main(void) {
	//char 배열
	char str[10] = "hello";
	std::cout << length(12345) << std::endl;
	std::cout << length(str) << std::endl;
	return 0;
}
int length(int n) {
	int cnt = 0;
	while (n > 0) {
		n /= 10;
		cnt++;
	}
	return cnt;
}
int length(char* str) {
	return strlen(str);
}
```

```C++
#include <iostream>
#include <string>
int length(int n);
int length(const char* str);
int main(void) {
	//문자열 상수
	std::cout << length(12345) << std::endl;
	std::cout << length("hello") << std::endl; //상수에 있어서 수정할 수 없다.
	return 0;
}
int length(int n) {
	int cnt = 0;
	while (n > 0) {
		n /= 10;
		cnt++;
	}
	return cnt;
}
//const는 상수, char*는 문자열 시작 주소 (포인터), str는 포인터 변수
int length(const char* str) {
	return strlen(str);
}
```
    