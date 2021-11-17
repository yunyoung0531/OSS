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
```CSS
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

```CSS
     cmd -a -b -o "xxx z yy" filename

     cmd -a -b -o "xxx z yy" -- filename

     cmd -ab -o xxx,z,yy filename

     cmd -ab -o "xxx z yy" filename

     cmd -o xxx,z,yy -b -a filename
```

2.3.1) 예제 2

다음의 쉘 스크립트는 입력 아규먼트(positional parameter)를 파싱하고 출력하는 예이다.

```CSS
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
```

getopts는 optstring에 포함되지 않은 옵션 문자를 만나는 경우 표준 에러 출력으로 에러 메시지를 출력한다.


2.3) 완화된 명령 구문 규칙에 따른 유의사항

다음과 같은 완화된 명령 구문 규칙( intro(1)참조)이 현재의 구현에서 허용되기는 하지만, 다음 버젼에서는 지원되지 않을 수도 있기 때문에 사용하지 않아야 한다. 위의 예에서 a와 b는 옵션이고 옵션 o는 옵션 아규먼트를 필요로 하는 아규먼트이다.


다음의 예는 옵션 아규먼트를 가지는 옵션은 다른 옵션들과 묶여져서는 안된다는 Rule 5를 위반하고 있다.
```CSS
     example% cmd - aboxxx filename
```

다음의 예는 옵션 아규먼트를 가지는 옵션의 다음에는 공백문자가 있어야 한다는 Rule 6를 위반하고 있다.
```CSS
     example% cmd - ab oxxx filename

     example% cmd - ab oxxx filename
```

쉘 변수 OPTIND의 값을 변경하거나 다른 아규먼트 셋을 파싱하는 경우, 결과는 예측할 수 없다.


2.5) 서브 쉘 또는 다른 실행환경에서  getopts의 동작시 유의사항

getopts는 현재 쉘의 실행 환경(shell execution environment)에 영향을 주기 때문에, 일반적으로 쉘의 내장 명령어로 제공된다. getopts간 서브쉘에서 호출되거나, 다음과 같이 별도의 유틸리티 실행 환경에서 호출되는 경우
```CSS
            (getopts abc value "$@")

            nohup getopts ...

            find . - exec getopts ... \;
```
이를 호출한 부모 쉘의 환경(caller's environment)에는 영향을 줄 수가 없다.


positional parameter가 변할 수는 있지만, 쉘 함수(shell function)는 호출하는 쉘과$OPTIND 쉘 변수를 공유한다는 점에 유의하여야 한다. 일반적으로 getopts를 사용하여 입력 아규먼트를 파싱하고자하는 쉘 함수는 시작시에 $OPTIND의 값을 저장하고 리턴하기 전에 $OPTIND의 값을 시작시 에 저장한 값으로 복구한다. 그러나 쉘 함수를 호출하는 쉘을 위하여 $OPTIND를 바꾸기를 원하는 경우도 있을 수 있다.


3. C 라이브러리 함수 getopt

getopt는 C 라이브러리 함수 getopt()의 형태로도 제공된다. getopt()함수를 C코드에서 사용하기 위해서는 다음과 같이 <stdlib.h> 헤더 파일을 include하여야 한다.
```C
     #include <stdlib.h>
```

3.1 함수 프로토타입
```C
     int getopt(int argc, char * const *argv,  const  char  *optstring);

     extern char *optarg;

     extern int optind, opterr, optopt;
```
3.2 함수 설명

getopt() 함수는 char **argv에서 char *optstring내에 있는 문자중 하나와 일치하는 옵션 문자를 리턴한다. getopt()함수는 명령 구문 표준에 있는 모든 규칙을 지원한다. 새로 작성되는 모든 명령어들은 명령 구문 표준을 준수하여야 하므로, 명령행에서 positonal parameter를 분석하고 유효한지를 검사하기 위해서는 사용자 명령어인 getopts,또는 C 라이브러리 함수인 getopt(), getsubopt()를 사용하여야 한다.

char *optstring은 getopt() 함수가 처리하여야할 옵션 문자들을 포함한다. 옵션 문자뒤에 콜론 ':'이 오면, 해당 옵션이 한 개의 옵션 아규먼트 또는 공백 문자로 분리되는 복수개의 옵션 아규먼트를 가지는 것을 나타낸다.

char *optarg는 getopt()함수의 리턴시에 옵션 아규먼트의 시작 위치를 가리키도록 설정된다.

getopt() 함수는 extern int optind에 다음에 처리할 아규먼트에 대한 char **argv내에서의인덱스를 저장한다( char **argv는 의미적으로 char *argv[]와 동등하다). extern int optind
는 getopt()함수의 최초 호출전에 1로 초기화된다.

모든 옵션이 처리되고 나면( 즉 옵션이 아닌 아규먼트를 만나게 되면), getopt()는 EOF를 리턴한다.

연속되는 두개의 하이픈으로 표시되는 특수 옵션 '--'를 사용하여 옵션의 끝을 나타낼수도 있다.

getopt() 함수가 '--'를 만나는 경우에도 EOF가 리턴된다. 특수 옵션 '--'는 '-'로 시작하는 옵션이 아닌 문자열을 처리하고자 할 때 유용하게 사용할 수 있다.


3.3) 리턴값

getopt() 함수가 char *optstring에서 지정되지 않은 옵션 문자를 만나거나 옵션 아규먼트를 가지는 옵션문자뒤에 옵션 아규먼트가 없는 경우에 표준 에러(stderr)에 에러 메시지를 출력하고 "?"를 리턴한다. extern int opterr을 0으로 설정하여 에러 메시지가 출력되지 않도록 할 수도 있다. 에러를 유발시킨 문자는 extern int optopt에 저장된다.

3.4 getopt() 함수 사용에 따른 유의 사항

getopt() 함수를 사용한 C코드가 -lintl로 링크되는 경우에, getopt()가 출력하는 메시지는

LC_message locale category로 지정된 국가 언어로 출력된다. 자세한 내용은 setlocale()

함수를 참조한다.

getopt() 함수는 옵션 아규먼트에 대한 것을 완벽하게 검사하지는 않는다. 예를 들어 옵션 문자열 optstring이 "a:b"이고 char **argv에  "-a -b"가 입력으로 사용된 경우,  getopt() 함수는 "-b"를 "-a"옵션에 대한 옵션 아규먼트로 처리한다.

아래의 예와 같이 옵션과 옵션 아규먼트를 함께 묶에서 사용하는 것은 명령 구문 표준을 위반한다.

cmd -abo filename

위의 예에서 a와 b는 옵션이고 o는 옵션 아규먼트를 필요로하는 옵션이며 filename은 옵션 o에 대한 옵션 아규먼트이다. 현재 버젼의 getopt()에서는 이러한 구문이 허용되기는 하지만, 차기 버전에서 이러한 구문이 지원되지 않을 수도 있으므로 다음과 같은 명령 구문 표준에 맞게 사용하여야 한다.

     cmd -ab -o filename.


3.4 getopt() 함수를 사용한 C 프로그램 예제

다음의 예제 코드는 C 프로그램에서 getopt() 함수를 사용하여 명령행 옵션을 처리하는

과정을 예시한 것으로 옵션 -a, -b와 옵션 아규먼트를 사용하는 -o옵션을 처리하는

코드이다.
```C
     #include <stdlib.h>

     #include <stdio.h>

     main (int argc, char **argv)

     {

        int c;

        extern char *optarg;

        extern int optind;

        int aflg = 0;

        int bflg = 0;

        int errflg = 0;

        char *ofile = NULL;


        while ((c = getopt(argc, argv, "abo:")) != EOF)

           switch (c) {

           case 'a':

              if (bflg)

                 errflg++;

              else

                 aflg++;

              break;

           case 'b':

              if (aflg)

                 errflg++;

              else

                 bflg++;

              break;

           case 'o':

              ofile = optarg;

              (void)printf("ofile = %s\n", ofile);

              break;

           case '?':

              errflg++;

           }

        if (errflg) {

           (void)fprintf(stderr,

              "usage: cmd [-a|-b] [-o <filename>] files...\n");

           exit (2);

            }

            for ( ; optind < argc; optind++)

          (void)printf("%s\n", argv[optind]);    return 0;

     }
```


