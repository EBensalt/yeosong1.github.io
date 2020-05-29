---
published: true
lang: 'ko'
---

### echo, 프롬프트 색상 바꾸기

~~~
echo -e "\x1B[32;1m""green and end""\x1B[m"
echo -e "\e[31mOutput as is.\e[m"
printf "\e[32mThis is green line.\e[m\n"
printf "\e[33;1m%s\n" 'This is yellow bold line.'
echo -e "\033[1;31m This is red text \033[0m"
~~~

- [검색1](http://www.kirknodeng.com/?p=489)
- [검색2](https://stackoverflow.com/questions/28782394/how-to-get-osx-shell-script-to-show-colors-in-echo)
