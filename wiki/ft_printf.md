## Tester
listed in 2020.
* [https://github.com/gavinfielder/pft.git](https://github.com/gavinfielder/pft.git)
* [https://github.com/cclaude42/PFT_2019](https://github.com/cclaude42/PFT_2019)
* [https://github.com/Mazoise/42TESTERS-PRINTF](https://github.com/Mazoise/42TESTERS-PRINTF)
* [https://github.com/Kwevan/PRINTF_TESTER](https://github.com/Kwevan/PRINTF_TESTER)
* [https://github.com/charMstr/printf_lover_v2](https://github.com/charMstr/printf_lover_v2)
* [https://github.com/AntoineBourin/printf-tester](https://github.com/AntoineBourin/printf-tester)


## 소개

* C에서 printf 함수가 제공하는 다양한 활용은 아주 좋은 프로그래밍 연습이 됩니다.
* 이 프로젝트는 다소 어렵습니다.
* C에서 [가변 함수](variadic_functions)를 사용할 수 있게 될 겁니다.
* 성공적인 ft_printf의 핵심은 체계적이고 확장성이 뛰어난 코드입니다..

## 일반 지침

## 필수 파트

* ft_printf 의 프로토타입은 이렇게 하세요 : int ft_printf(const char *, ...);
* 라이브러리의 printf 함수를 다시 짜는 것입니다. 
* 실제 printf처럼 버퍼 매니지먼트를 해서는 안됩니다!
* 다음 conversions(변환)을 매니지합니다. : c, s, p, d, i, u, x, X, %
* 다음 flags(플래그)조합을 매니지 합니다. : [-] [0] [ . ] [ * ] , [모든 컨버전스와 사용하는 최소 필드 너비]도..
* 실제 printf와 비교할 것입니다.
**👉[printf 포맷 보기](https://github.com/yeosong-00/42/wiki/printf-%ED%8F%AC%EB%A7%B7-%EC%8A%A4%ED%8A%B8%EB%A7%81)**

## 보너스 파트

* 필수 부분이 완벽하지 않은 경우 보너스에 대해 생각조차 하지 마십시오
* 모든 보너스를 할 필요는 없습니다
* 다음 변환 중 하나 이상을 관리하십시오. : nfge
* 다음 플래그 중 하나 이상을 관리하십시오. : l ll h hh
* 다음 플래그를 모두 관리합니다. : [ # ], [   ], [ + ] (응, 저 중에 하나는 공백 말하는 거임)
