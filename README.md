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

    arg  : 쉘 스크립트에 전달되는 positional parameter대신 분석하고자 하는 문자열


1.2) getopts의 동작 과정

getopts가 호출될 때마다, getopts는 쉘 변수 $name에 다음 옵션 문자를 저장하고, 쉘 변수 $OPTIND에 처리하여야 할 다음 아규먼트에 대한 인덱스를 저장한다. 쉘이 호출 될 때 마다, $OPTIND는 1로 초기화된다.

옵션에서 옵션-아규먼트(option-argument)가 필요한 경우, getopts는 옵션-아규먼트를 쉘 변수 $OPTARG에 저장한다. 분석과정 중에 옵션 아규먼트를 필요로 하는 옵션문자가 없거나, 옵션-아규먼트를 필요로 하는 옵션문자가 있지만 이에 대한 에옵션 아규먼트가 없는 경우에 $OPTARG는 unset된다.

optionstring 오퍼랜드에 포함되지 있지 않은 옵션 문자가 옵션 문자 위치에 있는 경우 name으로 지정된 쉘 변수는 물음표 '?'로 설정된다. 이 경우, optstring의 첫 문자가 콜론 ':'이라면, 쉘 변수 $OPTARG는 발견된 옵션 문자로 설정되며, 표준 에러 출력(stderr)에는 아무 것도 출력되지 않는다. 그 이외의 경우에 쉘 변수 $OPTARG는 unset되고 표준 에러 출력으로 diagonostic message가 출력된다. 이러한 상황은 호출하는 애플리케이션에 아규먼트가 주어지는진 과정에서 탐지된 에러로 볼 수는 있지만, getopts 처리과정에서의 에러는 아니다.

옵션 아규먼트가 없는 경우
l       optstring의 첫번째 문자가 콜론 ':'인 경우, name으로 지정된 쉘 변수는 콜론 ':'문자    로 설정되고, 쉘 변수 OPTARG는 발견된 옵션 문자로 설정된다.

l       optstring의 첫번째 문자가 콜론 ':'이 아닌 경우에는 name으로 지정된 쉘 변수는 물음표 ?'로 설정되고, 쉘 변수 OPTARG는 unset되면서 표준 에러 출력으로 diagonostic message가 출력된다. 이러한 상황은 호출하는 애플려케이션에 아규먼트가 주어지는 과정에서 탐지되는 에러로 볼 수는 있지만, getopts 처리과정에서의 에러는 아니다. diagonostic messge가 위에서 기술한 것처럼 출력되기는 하지만 exit status는 0으로 설정된다.

옵션의 마지막에 도달한 경우, getopts는 1이상의 리턴값을 가지고 exit한다. 쉘 변수 OPTIND 값은 옵션-아규먼트가 아닌 첫번째 옵션에 대한 인덱스로 설정된다. 최초의 -- 아규먼트 이전에 다른 non-option-argument가 없는 경우에 -- 아규먼트는 옵션-아규먼트로 간주된다. 최초의 --아규먼트 이전에 다른 non-option-argument가 있고 다른 non-option-argument가 없는 경우에는 최초의 -- 아규먼트는 $#+1의 값을 가지는 아규먼트로 간주되며 name변수의 값은 물음표 '?'로 설정된다.  다음의 조건중 하나를 만족하는 경우 옵션의 끝이 된다.

         . 스페셜 옵션 --

         . -로 시작하지 안는 아규먼트를 발경한 경우

         . 에러가 발생한 경우

1.3) getopt와 관련한 쉘 환경 변수에 따른 유의사항

l       쉘 변수 OPTIND와 OPTARG는 getopts를 호출한 caller에 로컬이며 기본적으로 export되지 않는다.

l       쉘변수 $name, $OPTIND, $OPTARG는 현재 쉘의 실행 환경에 영향을 미친다.

l       애플리케이션에서 $OPTIND의 값을 1로 설정한 경우, 새로운 파라메터 셋( 현재 positional parameter 또는 새로운 arg값)이 사용될 수도 있다. 단일 쉘에서 다음과 같이 getopts를 여러번 호출할 경우에 결과는 예측할 수 없다.

-       모든 getopts 호출에서 동일하지 않은 파라메터 (positional paramet 또는 arg 아규먼트)를 사용하는 경우

-       1이 아닌 값으로 OPTIND값을 변경하여 사용하는 경우


2) 본쉘(Bourne shell)의 내장 명령어로서의 getopts

getopts는 본쉘(Bourne shell)의 내장 명령어로써 positional parameter $1 ~ $#을 파싱하고 유효한 옵션인지 검사한다. 본 쉘에 내장 명령어로써의 getopts는 명령 구문 표준(command syntax standard) 의 적용가능한 규칙을 지원하며, getopt 명령어를 대신하여 사용되어야 한다. 명령 구문 표준에 대한 내용은 intro(1)을 참조한다.

2.1.1)  optstring

getopts를 호출하는 유틸리티가 인식할 수 있는 옵션 문자들을 포함하고 있는 문자열. 문자뒤에 콜론 ':'이 오면, 해당 옵션은 옵션 아규먼트를 가지며 이 옵션 아규먼트는 별도의 아규먼트로 제공되야 한다. 애플리케이션은 하나의 옵션 문자와 이 옵션 문자의 아규먼트를 별도의 아규먼트로 명시하여야 하지만, getopts는 애플리케이션에서 별도의 아규먼트로 옵션 아규먼트를 지정하는 것과는 상관없이 옵션 아규먼트를 필요로 하는 옵션 문자뒤에 오는 문자들을 옵션 아규먼트로 해석해 버린다.

getopts 호출시에 옵션 아규먼트가 별로의 아규먼트로 제공되지 않는다면 명시적인 null 옵션 아규먼트를 구분할 피요는 없다. getopt(3)을 참조바람. 물음표 '?'와 콜론 ':'은 애플리케이션에서 옵션 구분 문자로 사용할 수 없다. 또한 숫자나 영문자가 아닌 옵션 문자를 사용할 경우의 실행 결과는 예측할 수 없다. 옵션 아규먼트가 별도의 아규먼트로 제공되지 않는 경우, 쉘 변수 $OPTARG의 값은 옵션문자와 -로 없어지게 된다. 옵션 문자가 없거나 옵션 아규먼트가 생략된 경우에 getopts의 동작은 optstring 의 최초 문자에 의해 결정된다.


2.1.2) name

파싱과정에서 옵션 문자를 저장하기 위하여 사용되는 쉘 변수의 이름

2.1.3 arg

getopts는 기본적으로 쉘 프로시져 호출시에 전달되는 positional parameter를 파싱한다. 별도의 아규먼트 arg가 주어진 경우에는 postional parameter대신에 arg로 주어진 문자열을 파싱한다.


2.2) getopts의 동작 과정

getopts는 호출될때 마다, 쉘 변수 $name에 다음 옵션을 저장하고 다음 처리할 옵션에 대한 인덱스를 쉘 변수 $OPTIND에 저장한다. 쉘 또는 쉘 스크립트가 호출될 때마다, 쉘 변수 $OPTIND의 값은 1로 초기화된다.

옵션이 옵션-아규먼트를 필요로 할 경우, getopts는 옵션-아규먼트를 쉘 변수 $OPTARG에 저장한다.

올바르지 않은 옵션을 만나는 경우에는 물음표 '?'가 쉘 변수 $name에 저장된다.

옵션의 끝에 도달한 경우, getopts는 0이 아닌 exit status로 종료한다. 특수 옵션 --가 옵션을 끝을 나타내기 위하여 사용될 수 있다.

기본적으로, getopts는 positional parameter를 파싱한다. 만일  별도의  아규먼트 arg가 getopts에 주어진 경우, getopts는 positional parameter대신에 별도로 주어진 아규먼트 arg를 파싱한다.

/usr/lib/getoptcvt는 filename내의 쉘 스크립트를 읽어들여, 이를 getopt대신에 getopts를 사용하도록 변환한 후 결과를 표준 출력에 출력한다.

모든 새로 작성되는 명령어는 intro(1)에서 기술된 명령 구문 표준(command syntax standard)을 준수하기 위하여 getopts또는 getopt를 사용하여 positional parameters를 파싱하고 해당 명령어에 유효한 옵션인지를 검사하여야 한다.

getopts수행후에는 다음의 종료값이 리턴된다.


     0         optstring에 의해 지정되거나 지정되지 않은 옵션이 발견됨


     >0        옵션의 끝 또는 에러가 발생


2.3) 예제

2.3.1) 예제 1

다음의 코드는 optstring은 "abo:"으로 하고 c를 다음 옵션을 저장하기 위한 쉘 변수로 하여 프로그래밍한 예이다.

요약 설명

"abo:"   : 옵션 문자로 사용될 문자들을 지정한 optstring으로

           -a, -b, -o의 옵션 문자가 사용될수 있으며 -o옵션은 옵션 아규먼트를 가진다.

$c       : 다음 옵션을 위한 사용자 쉘 변수

$OPTARG  : 옵션이 옵션-아규먼트를 필요로 하는 경우 옵션-아규먼트가 저장되는 쉘 내장 변수


      예제 코드)
```C
      while getopts abo: c

      do

           case $c in

              a | b)   FLAG=$c;;

              o)       OARG=$OPTARG;;

              \?)      echo $USAGE

                       exit 2;;

           esac

     done

     shift `expr $OPTIND - 1`
```


위의 코드는 다음의 모든 명령행을 동등한 것으로 취급한다.


     cmd -a -b -o "xxx z yy" filename

     cmd -a -b -o "xxx z yy" -- filename

     cmd -ab -o xxx,z,yy filename

     cmd -ab -o "xxx z yy" filename

     cmd -o xxx,z,yy -b -a filename


2.3.1) 예제 2

다음의 쉘 스크립트는 입력 아규먼트(positional parameter)를 파싱하고 출력하는 예이다.


     aflag=

     bflag=

     while getopts ab: name

     do

          case $name in

          a)      aflag=1;;

          b)      bflag=1

                  bval="$OPTARG";;

          ?)     printf "Usage: %s: [-a] [-b value] args\n"  $0

                 exit 2;;

          esac

     done

     if [ ! -z "$aflag" ]; then

        printf "Option -a specified\n"

     fi

     if [ ! -z "$bflag" ]; then

          printf 'Option -b "%s" specified\n' "$bval"

     fi

     shift $(($OPTIND - 1))

     printf "Remaining arguments are: %s\n" "$*"


getopts는 optstring에 포함되지 않은 옵션 문자를 만나는 경우 표준 에러 출력으로 에러 메시지를 출력한다.


2.3) 완화된 명령 구문 규칙에 따른 유의사항

다음과 같은 완화된 명령 구문 규칙( intro(1)참조)이 현재의 구현에서 허용되기는 하지만, 다음 버젼에서는 지원되지 않을 수도 있기 때문에 사용하지 않아야 한다. 위의 예에서 a와 b는 옵션이고 옵션 o는 옵션 아규먼트를 필요로 하는 아규먼트이다.


다음의 예는 옵션 아규먼트를 가지는 옵션은 다른 옵션들과 묶여져서는 안된다는 Rule 5를 위반하고 있다.

     example% cmd - aboxxx filename


다음의 예는 옵션 아규먼트를 가지는 옵션의 다음에는 공백문자가 있어야 한다는 Rule 6를 위반하고 있다.

     example% cmd - ab oxxx filename

     example% cmd - ab oxxx filename


쉘 변수 OPTIND의 값을 변경하거나 다른 아규먼트 셋을 파싱하는 경우, 결과는 예측할 수 없다.


