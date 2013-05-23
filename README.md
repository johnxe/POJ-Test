POJ-Test
========

POJ ACM Problems 2406

/*  write by xingming
 *  time:2012年10月19日15:51:53
 *  for: test regex
 *  */

#include <regex.h>
#include <iostream>
#include <sys/types.h>
#include <stdio.h>
#include <string>
#include <sys/time.h>

using namespace std;
const int times = 1000000;

bool regcompare(char* str, char* pattern)
{
  const size_t nmatch = 20;
	regmatch_t pm[20];
	int z ;
	regex_t reg;
	regcomp(&reg,pattern,REG_EXTENDED|REG_NOSUB);
	for(int i = 0 ; i < times; ++i)
	{
		z = regexec(&reg,str,nmatch,pm,REG_NOTBOL);
		if(z!=REG_NOMATCH)
		{
			regfree(&reg);
			return true;
		}
	}
	regfree(&reg);
	return false;
}

int arr[1000000];
int alen;

void getsubarr(int a)
{
	alen = 0;
	for(int i=1; i<=a; i++)
	{
		if(a % i == 0)
		{
			arr[alen++] = i;
		}
	}
}

int main()
{
	string str;
	while(cin>>str)
	{
		if(str.compare(".") == 0)
		{
			return 0;
		}
		getsubarr(str.length());
		for(int i=0; i<alen; i++)
		{
			string pattern;
			pattern = str.substr(0, arr[i]);
			int n = str.length()/arr[i];

			char buf[10];
			sprintf(buf, "%d", n);
			pattern = "(" + pattern + "){" + buf +"}";
			if(regcompare(&str[0], &pattern[0]))
			{
				cout<<n<<endl;
				//printf("%d\n", str.length()/arr[i]);
				break;
			}
		}
	}
    return 0 ;
}
