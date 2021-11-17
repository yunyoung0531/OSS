# OSS
오픈소스SW개론 첫번째 과제 조선대학교 컴퓨터공학과 20203215 최윤영

**********************************

## getopts
1) 사용자 명령어로써의 getopts 
getopts 유틸리티는 파라메터들의 리스트에서 옵션(options)과 옵션-파라메터(option-parameter)를 추출하는데 사용한다.

1.1) 사용법

getopts의 사용 방법은 다음과 같다.

/usr/bin/getopts optstring name  [ arg ...  ]


    optstring : 옵션으로 사용될 문자들로 구성된 문자열

    name : getopt가 옵션 분석과정에서 분석된 옵션 문자열을 저자하는 쉘 변수

    arg  : 쉘 스크립트에 전달되는 positional parameter대신 분석하고자 하는

           문자열
