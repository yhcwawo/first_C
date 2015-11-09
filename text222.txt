#define _CRT_SECURE_NO_WARNINGS
//scanf문을 사용하므로 경고메시지가 나오지 않도록 한다.

//헤더파일 정의
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

//스택의 초기값 설정
int OperatorTop = 0;  
//연산자 스택의 초기값
int ValueTop = 0;
//값의 스택의 초기값

char OperatorStack[100]; //스택 연산자 배열
char ValueStack[100];  //스택 값 배열




int IsEmptyValueStack()
{
	if (ValueTop == 0)
		return 1;
	else return 0;
}
//값 스택 배열이 비어있는 상태인지 확인하는 함수


int Is_Empty_OperatorStack()
{
	if (OperatorTop == 0)
		return 1;
	else return 0;
}
//연산자 스택 배열이 비어있는 상태인지 확인하는 함수



void OperatorPush(char opr)
{
	OperatorStack[OperatorTop++] = opr;
}
//연산자 스택에 연산자를 입력하는 함수
//입력시 연산자의 초기값을 1 증가시킨다.


void ValuePush(int value)
{
	ValueStack[ValueTop++] = value;
}
//값 스택에 값을 입력하는 함수
//입력시 값의 초기값을 1 증가시킨다.


char OperatorPop()
{
	return OperatorStack[--OperatorTop];
}
//연산자를 꺼내기 위한 함수
//결과 값으로는 가장 마지막에 있는 연산자가 반환된다.



int ValuePop()
{
	return ValueStack[--ValueTop];
}

//값을 꺼내기 위한 함수
//결과 값으로는 가장 마지막에 있는 값이 반환된다.







int Priority(char opr1, char opr2)
{
	if (opr1 == '*' || opr1 == '/'){
		if (opr2 == '+' || opr2 == '-')
			return 1;
		else return 0;
	}
	else return 0;
}
//곱셈이나 나눗셈은 덧셈과 뺄셈보다 우선시 되어야 한다.
//그에 따른 순서를 정해주는 함수




int Calculate(int value1, int value2, char opr)
{
	switch (opr){
	case'+':
		value1 = value1 + value2;
		break;
	case'*':
		value1 = value1 * value2;
		break;
	case'/':
		value1 = value2 / value1;
		break;
	}

	return value1;
}
//값 2개를 입력받고 연산자를 입력받았을때 산술계산을 하는 함수
//입력은 계산 할 값 2개와 char형 연산자를 입력받는다.


int Calculate2(int value1, int value2, char opr)
{
	switch (opr){
	case'+':
		value1 = value1 + value2;
		break;
	case'*':
		value1 = value1 * value2;
		break;
	case'/':
		value1 = value2 / value1;
		break;
	case'-':
		value1 = value1 - value2;
		break;
	}

	return value1;
}
//Caluetate함수와 비슷하지만 추가 연산시에는 뺄셈 연산시 특이사항을 고려할 필요가 없으므로
//switch에 뺄셈 파트를 추가하였다.
//테이블값과 추가연산자, 추가연산값을 인풋으로 받고 결과를 반환하는 함수이다.



void main()
{
	
	int i=0;  //기본적인 카운트를 위한 변수
	int len;  //입력받은 문자열의 길이를 저장하는 변수
	int check_signal = 0;  
	int value1, value2; //연산을 할때 값 2개를 서로 비교해야 하므로 값에 대한 변수를 2개 정의해준다.
	int table; //모든 연산이 끝났을때 결과값을 저장하는 변수
	int count1 = 0;
	int count2 = 0;

	char buf[100]; //strcpy로 입력받은 문자열을 저장하는 배열
	char input[100];  //처음에 입력받은 문자열을 저장하는 배열
	char opr; //연산자 변수
	char opr2[100]; //추가 계산을 위해 연산자를 입력 받는 배열
	char A; //연산자 관계변수
	char temp;//괄호 연산을 위한 변수
	
	int nValue;  //모든 연산이 끝난후에 추가연산 시 추가 연산할 값을 저장하는 변수


	printf("----------------------------------------------\n");
	printf("\n스택을 활용한 계산을 시작합니다.\n");
	printf("\n----------------------------------------------\n");
  	printf("괄호에 주의하여 계산 할 수식을 입력해주세요 : ");
	scanf("%s", &input);
	//괄호를 포함한 상태의 문자열 형태 수식을 입력받아 input에 저장한다.

	

	strcpy(buf, input);
	//input에 저장한 문자열을 buf 배열로 복사한다.

	len = strlen(buf);
	//buf 배열에 있는 문자열의 길이를 len 변수에 저장한다.

	

	//괄호의 쌍이 맞는지 확인하기 위한 계산식
	//'('와 ')'의 갯수를 세서 서로의 쌍이 맞는지 체크한다.
	for (i = 0; i < len; i++)
	{

		if (input[i] == '(')
		{
			count1 = count1 + 1;
		}


		if (input[i] == ')')
		{
			count2 = count2 + 1;
		}
	}

		if (count1 != count2)
		{
			printf("괄호의 쌍이 맞지 않습니다.\n");
		}
		else
			printf("괄호의 쌍이 일치합니다.\n");

	i=0;
	//i 값을 다시 초기화
		

	while (i<len){
		A = buf[i++];
		//반복문은 문자열의 길이 만큼 반복한다.
		//A변수는 문자열의 첫 번째 부터 진행한다.

		//현재 체크하고 있는 문자열의 번지가 공백인 경우
		if (A == ' ')  continue;
		else if ('0' <= A && A <= '9')
		{
			//check_signal의 초기값은 0으로 잡혀있다.
			//A변수가 0~9의 값을 가질 경우 체크
			//0~9사이의 값이라고 판단될 경우 값스택 배열에 삽입한다.
			if (check_signal != 1) ValuePush(A - '0');   
			else
			{
				check_signal = 0;
				ValuePush(-(A - '0'));
			}
		}

		//체크하고 있는 문자열이 연산자인 경우이다.
		//뺄셈 연산이 있는 경우 signal을 1로 변경하고 원래 연산자값을 +로 변경한다.
		else 
		if (A == '+' || A == '-' || A == '*' || A == '/'){
			if (A == '-')
			{
				check_signal = 1; A = '+';
			}
			

			if (Is_Empty_OperatorStack())
				OperatorPush(A);
			//연산자 스택이 비었다면 연산자를 넣어준다.


			else{
				opr = OperatorPop();
				

				//연산자 스택에 연산자가 있으면 A에 저장한 연산자와 현재 연산자 스택값과 어느 연산자를 먼저 할지 비교한다.
				if (Priority(opr, A))
				{
					value2 = ValuePop();
					value1 = ValuePop();

					value1 = Calculate(value1, value2, opr);
					ValuePush(value1);
					OperatorPush(A);
				}

				//기존 값 스택에 있는 값을 먼저 계산 해야되면 계산해야할 값을 pop해서 결과를 값 스택에 다시 삽입한다.
			    else{
					OperatorPush(opr);
					OperatorPush(A);
				}
			}
		}

		// 괄호가 있을때 연산을 하는 코드이다.
		//'('가 나타나면 연산자를 꺼내고 ')'전까자의 값을 계산함수로 계산한다.
		else if (A == '('){
			OperatorPush(A);
		}
		else if (A == ')'){
			do{
				temp = OperatorPop();
				if (temp != '('){
					value2 = ValuePop();
					value1 = ValuePop();

					value1 = Calculate(value1, value2, temp);
					ValuePush(value1);
				}
			} while (temp != '(');
		}

	}

	//스택에 있는 값이 없어질때까지 계산을 반복한다.
	while (!Is_Empty_OperatorStack()){
		value1 = ValuePop();
		value2 = ValuePop();
		opr = OperatorPop();
		value1 = Calculate(value1, value2, opr);
		ValuePush(value1);
	}

	
	
	//마지막에 pop된 값을 저장한다.
	table = ValuePop();
	//이전 값을 테이블에 저장하기 위해서 값의 pop값을 변수에 먼저 저장한다.


	printf("%s = %d\n", buf, table);
	//최종 결과출력문

	int go;
	//뒤에 수식계산을 추가로 진행할지 여부를 저장하는 변수

	printf("수식계산을 계속 하시겠습니까?(1=yes,0=no)");
	scanf("%d", &go);
	//go 변수에 결과를 입력받고
	//switch문을 통해 코드를 진행한다.

	
	nValue = 0;
	//입력받을 값을 저장하기 위한 변수 초기화
	switch (go)
	{
	case 1:
		//1을 입력할 경우 추가 연산모드 진입
		
		printf("이전 연산의 값은 %d 입니다.\n",table);

		printf("-------------------------------------------------------\n  ");
		printf("계산하려는 연산자를입력해주세요(ex:+,*,/)   :   ");
		scanf("%s", &opr2);


		printf("계산하려는 값을 입력해주세요(ex: 12,30,1)   :   ");
		scanf("%d", &nValue);
		printf("\n-------------------------------------------------------\n  ");

		printf("\n추가 연산 결과는 %d 입니다.", Calculate2(table, nValue, opr2[0]));
	
		printf("\n추가 연산 완료");



		break;
	case 0:
		printf("계산을 종료합니다.");
		break;

		//계산기 종료
	default:
		printf("1과 0으로만 입력해주세요.");
		
	}





}