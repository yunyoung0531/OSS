![Anurag's GitHub stats](https://github-readme-stats.vercel.app/api?username=yunyoung0531&show_icons=true&theme=radical)

# OSS
___오픈소스SW개론 첫번째 과제 조선대학교 컴퓨터공학과 20203215 최윤영___

**********************************

## getopt & getopts
### getopt
1) getopt에 대한 간단한 설명
* 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석한다. 
* 모든 토큰을 읽은 후 또는 특수 토큰 -(더블 하이픈)이 발생하는 경우 처리를 완료한다.
* 토큰이 플래그와 일치하는 데 실패하는 경우 getopt 명령은 메시지를 표준 오류에 기록한다.

2) getopt 구문
* getopt Format Tokens

3) getopt() 함수 사용에 따른 유의 사항
     getopt() 함수를 사용한 C코드가 -lintl로 링크되는 경우에, getopt()가 출력하는 메시지는 LC_message locale category로 지정된 국가 언어로 출력된다. 
     자세한 내용은 setlocale() 함수를 참조한다.

     getopt() 함수는 옵션 아규먼트에 대한 것을 완벽하게 검사하지는 않는다. 
     예를 들어 옵션 문자열 optstring이 "a:b"이고 char **argv에  "-a -b"가 입력으로 사용된 경우,  getopt() 함수는 "-b"를 "-a"옵션에 대한 옵션 아규먼트로 처리한다.

     아래의 예와 같이 옵션과 옵션 아규먼트를 함께 묶에서 사용하는 것은 명령 구문 표준을 위반한다.

          cmd -abo filename

     위의 예에서 a와 b는 옵션이고 o는 옵션 아규먼트를 필요로하는 옵션이며 filename은 옵션 o에 대한 옵션 아규먼트이다. 현재 버젼의 getopt()에서는 이러한 구문이 허용되기는 하지만, 차      기 버전에서 이러한 구문이 지원되지 않을 수도 있으므로 다음과 같은 명령 구문 표준에 맞게 사용하여야 한다.

          cmd -ab -o filename

4) getopt() 함수를 사용한 C 프로그램 예제
     다음의 예제 코드는 C 프로그램에서 getopt() 함수를 사용하여 명령행 옵션을 처리하는 과정을 예시한 것으로 옵션 -a, -b와 옵션 아규먼트를 사용하는 -o옵션을 처리하는 코드이다.
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

### getopts
1) getopt에 대한 간단한 설명
* 매개변수 리스트에서 옵션 및 옵션 인수를 검색하는 Korn/POSIX 쉘 내장 명령이다.
* OptionString의 문자 뒤에는 :(콜론)이 오면 옵션에 인수가 있는 것으로 간주된다.
* 파라미터들의 리스트에서 옵션(options)과 옵션-파라메터(option-parameter)를 추출하는데 사용한다.

2) getopts 사용법
    > /usr/bin/getopts optstring name  [ arg ...  ]

    _optstring_ : 옵션으로 사용될 문자들로 구성된 문자열

    _name_ : getopt가 옵션 분석과정에서 분석된 옵션 문자열을 저자하는 쉘 변수

    _arg_  : 쉘 스크립트에 전달되는 positional parameter대신 분석하고자 하는 문자열

3) getopts의 동작 과정
```getopts는 호출될때 마다, 쉘 변수 $name에 다음 옵션을 저장하고 다음 처리할 옵션에 대한 인덱스를 쉘 변수 $OPTIND에 저장한다. 
쉘 또는 쉘 스크립트가 호출될 때마다, 쉘 변수 $OPTIND의 값은 1로 초기화된다.
옵션이 옵션-아규먼트를 필요로 할 경우, getopts는 옵션-아규먼트를 쉘 변수 $OPTARG에 저장한다.
올바르지 않은 옵션을 만나는 경우에는 물음표 '?'가 쉘 변수 $name에 저장된다.
옵션의 끝에 도달한 경우, getopts는 0이 아닌 exit status로 종료한다. 특수 옵션 --가 옵션을 끝을 나타내기 위하여 사용될 수 있다.
기본적으로, getopts는 positional parameter를 파싱한다.
만일  별도의  아규먼트 arg가 getopts에 주어진 경우, getopts는 positional parameter대신에 별도로 주어진 아규먼트 arg를 파싱한다.
/usr/lib/getoptcvt는 filename내의 쉘 스크립트를 읽어들여, 이를 getopt대신에 getopts를 사용하도록 변환한 후 결과를 표준 출력에 출력한다.
모든 새로 작성되는 명령어는 intro(1)에서 기술된 명령 구문 표준(command syntax standard)을 준수하기 위하여 getopts또는 getopt를 사용하여 positional parameters를 파싱하고 해당 명령어에 유효한 옵션인지를 검사하여야 한다.
getopts수행후에는 다음의 종료값이 리턴된다.
```
|항목|설명|
|---|---------|
|0|optstring에 의해 지정되거나 지정되지 않은 옵션이 발견됨|
|>0|옵션의 끝 또는 에러가 발생|

## sed & awk

### awk
#### awk에 대한 간단한 설명
* 자료 처리 및 리포트 생성에 사용하는 프로그래밍 언어
* 입력 데이터로 표준입력이나 여러개의 파일 또는 다른 프로세스의 결과를 사용할 수 있다.
* 데이터를 조작할 수 있기때문에 shell script나 소규모 데이터베이스 관리에서 중요하게 사용된다.
* 사용자가 지정한 패턴 검색이나 특별한 작업을 수행하기 위해 파일을 행 단위로 조사
* 특정한 작업(action)이 정의되지 않은 패턴에 대해서는 단지 출력만 한다.
* 패턴없이 action이 정의도면 해당 action에 의해 지정된 모든 행에 대해서 처리가 이루어진다.

#### awk 형식(form)
* awk 'pattern' [파일명]
* awk '{action}' [파일명] -->액션으로 보통 print를 사용한다.
* awk 'pattern {action}' [파일명]

#### awk 명령에 사용되는 NR , NF 변수 와 옵션
* NR 변수 - 하나의 레코드를 처리한 후 1이 증가하는 변수. (즉, 해당 패턴이 몇 번째 행에 있는지 나타내 주는 변수)
* NF 변수 - 필드의 개수를 출력하는 변수
* -F 옵션 - 공백문자를 필드 구분자로 사용하지 않고 필드 구분자를 무엇으로 사용할지 지정하는 옵션.
* ~ 와 ! - ~는 특정 레코드나 필드내에서 일치하는 정규 표현식 패턴이 존재하는지 검사.

### sed (stream editor)
#### sed에 대한 간단한 설명
* 파일 또는 입력을 한 번에 한 행씩 처리하여 화면으로 출력한다.
* 파일의 마지막행까지 처리를 완료하면 종료한다.
* 직접 파일을 수정하지 않고 메모리의 임시기억장소(버퍼)에 저장하고 편집한다.

#### sed 형식(form)
* sed 'command' [filename]
* sed'(value)' member

#### option
|옵션|내용|
|---|------|
|-n|해당 패턴 or 실행 결과만을 출력 (기본적으로 출력되는 모든 행을 막는다.)|
|-e|다중 편집을 가능하게 해준다.|

#### sed 명령어 FORM(' '안에서 쓰인다.)
|형식|내용|
|---|------|
|p|print 명령 사용자가 지정한 행을 출력 _ex) '1,3p' -> 1행부터 3행까지 출력_|
|d|delete명령 사용자가 지정한 행을 삭제 _ex) '3d' -> 3번째 행을 삭제_|
|r|read명령 사용자가 지정한 행을 읽어온다 _ex) '3r phone' [패턴검색파일] --> 패턴검색할 파일의 3번째 행에 phone의 내용을 읽어와 출력._|
|w|write명령 사용자가 지정한 행에 쓴다. _ex) '/패턴/w [쓰여질파일]' [패턴검색파일] --> 패턴에 일치하는 행을 검색하여 [쓰여질 파일]에 쓴다._|
|a\ |append명령 검색된 패턴 아래 행에 내용을 덧붙인다. _ex) '/패턴/a\내용' [파일명] --> 패턴에 일치하는 행의 다음에 내용을 삽입하여 출력._|
|i\ |insert명령 검색된 패턴 위에 내용을 덧붙인다. a\와 사용법은 동일.|
