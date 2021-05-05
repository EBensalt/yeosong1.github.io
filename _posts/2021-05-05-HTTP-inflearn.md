---
published: false
---

# HTTP 기본 지식 정리
- 인프런 김영한 님 강의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/)를 뼈대로 정리

## 인터넷 통신
IP(인터넷 프로토콜)
TCP, UDP
PORT
DNS

## URI와 웹 브라우저 요청 흐름
URI
웹 브라우저 요청 흐름

## HTTP 기본
모든 것이 HTTP
클라이언트 서버 구조
Stateful, Stateless
비 연결성(connectionless)
HTTP 메시지
HTTP 메서드
HTTP API를 만들어보자
HTTP 메서드 - GET, POST
HTTP 메서드 - PUT, PATCH, DELETE
HTTP 메서드의 속성

## HTTP 메서드 활용
클라이언트에서 서버로 데이터 전송
HTTP API 설계 예시

## HTTP 상태코드
HTTP 상태코드 소개
2xx - 성공
3xx - 리다이렉션1
3xx - 리다이렉션2
4xx - 클라이언트 오류, 5xx - 서버 오류

## HTTP 헤더1 - 일반 헤더
HTTP 헤더 개요
표현
콘텐츠 협상
전송 방식
일반 정보
특별한 정보
인증
쿠키

## HTTP 헤더2 - 캐시와 조건부 요청
캐시 기본 동작
검증 헤더와 조건부 요청1
검증 헤더와 조건부 요청2
캐시와 조건부 요청 헤더
프록시 캐시
캐시 무효화
