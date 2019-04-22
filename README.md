# 笔试题

请完成以下笔试题，可以使用自己擅长的语言来编写，通过 github pull request 提交代码。

1. 编写一个递归版本的 reverse(s) 函数(或方法), 以将字符串s倒置。
#include<stdio.h>

#define MAXLINE 1000

int getline(char s[], int lim);
void reverse(char s[], int l);

/*
编写一个递归版本的 reverse(s) 函数(或方法),以将字符串s倒置
*/
main()
{
	char line[MAXLINE];   //当前行
	int len;            //当前行长度

	while ((len = getline(line, MAXLINE)) > 0)
	{
		reverse(line, len);
		printf("%s\n", line);
	}
	return 0;}

//翻转字符串
void reverse(char s[], int l)
{

	char t[MAXLINE];
	int i = l;
	for (int j = 0; j < l; j++)
	{
		t[j] = s[i - 2];
		i--;
	}
	for (int k = 0; k<l; k++)
	{
		s[k] = t[k];
	}
}

//获取输入行（此段为标准答案中的函数）
int getline(char s[], int lim)
{

	int c, i, j;

	j = 0;
	for (i = 0; (c = getchar()) != EOF && c != '\n'; ++i)
	if (i < lim - 2)
	{
		s[i] = c;
		++j;
	}
	if (c == '\n')
	{
		s[j] = c;
		++i;
		++j;
	}
	s[j] = '\0';
	return i;
}


2. 编写程序 expr，以计算从命令行输入的逆波兰表达式的值，其中每个运算符或操作数用一个单独的参数表示。例如，命令
expr 2 3 4 + *

#include<stdio.h>
#include<ctype.h>
int getfloat(char* str, double* store){

	while (isspace(*str))
		str++;
	int ch, sign;
	double power = 1.0;
	ch = *str++;
	sign = ch == '-' ? -1 : 1;
	if (ch == '+' || ch == '-')
		ch == *str++;
	for (*store == 0.0; isdigit(ch); ch = *str++)
	{
		*store = *store*10.0 + ch - '0';
	}
	if (ch == '.')
		ch = *str++;
	for (; isdigit(ch); ch = *str++)
	{
		*store = *store*10.0 + ch - '0';
		power *= 10.0;
	}
	if (ch == 0 || isspace(ch) || ch == EOF)
	{
		*store /= power;
		*store *= sign;
		return 1;
	}
	else
		return -1;
		}
	
//expr.c
#define MAX_NUM 100
double stack[MAX_NUM];
extern int getfloat(char* str, double*);

int main(int argc, char* argv[]){

	if (argc < 2) {
		printf("usage: expr num1 num2 operator ...\n");
		return 1;
	}
	else {
		int stackp = 0;
		int ch = 0;
		double num1, num2, ans;
		while (--argc >= 1) {
			ch = (*++argv)[0];
			if (ch == '-'){
				if (!isdigit(ch = (*argv)[1])) {
					//是负号
					if (stackp >= 2) {
						num2 = stack[--stackp];
						num1 = stack[--stackp];
						stack[stackp++] = num1 - num2;
						continue;
					}
					else {
						printf("Error: at least two numbers are needed before operator '-'\n");
						return -1;
					}
				}
			}
			if (isdigit(ch)) {
				if (stackp < 0) {
					printf("stack is underflowed!\n");
					return -2;
				}
				else if (stackp >= MAX_NUM) {
					printf("stack is overflowed!\n");
					return -3;
				}
				else if (getfloat(*argv, &stack[stackp++]) == -1) {
					printf("wrong argument to get number: %s\n", *argv);
					return -4;
				}
			}
			else if (ch == '+' || ch == '*' || ch == '/' || ch == '%') {
				if (stackp >= 2) {
					num2 = stack[--stackp];
					num1 = stack[--stackp];
					switch (ch) {
					case '+':
						ans = num1 + num2;
						break;
					case '*':
						ans = num1*num2;
						break;
					case '/':
						if (num2 == 0.0) {
							printf("error zero divisor: %f / %f\n", num1, num2);
							return -5;
						}
						ans = num1 / num2;
						break;
					case '%':
						ans = (int)num1 % (int)num2;
						break;
					}
					stack[stackp++] = ans;
				}
				else {
					printf("Error: at least two numbers are needed before operator '%c'\n", ch);
					return -6;
				}
			}
			else {
				printf("wrong arguments for %c,usage: expr num1 num2 operator ...\n", ch);
				return -7;
			}
		}
		if (stackp == 1) {
			printf("%.4f\n", stack[0]);
		}
		else {
			printf("the format of input is wrong!\n");
		}
	}
	return 0;
}


3. 用归并排序将3，1，4，1，5，9，2，6 排序。

#include <stdio.h>
#include <stdlib.h>
#define N 8

void merge(int arr[], int low, int mid, int high)
{

	int i, k;
	
	int *tmp = (int *)malloc((high - low + 1)*sizeof(int));
	
	int left_low = mid;
	int left_high = mid;
	int right_low = mid + 1;
	int right_high = high;

	for (k = 0; left_low <= left_high && right_low <= right_high; k++)
	{  // 比较两个指针所指向的元素
		if (arr[left_low] <= arr[right_low])
		{
			tmp[k] = arr[left_low++];
		}
		else
		{
			tmp[k] = arr[right_low++];
		}
	}

	if (left_low <= left_high)
	{
		//若第一个序列有剩余，直接复制出来粘到合并序列尾
		//memcpy(tmp+k, arr+left_low, (left_high-left_low+l)*sizeof(int));
		for (i = left_low; i <= left_high; i++)
			tmp[k++] = arr[i];
	}

	if (right_low <= right_high)
	{
		//若第二个序列有剩余，直接复制出来粘到合并序列尾
		//memcpy(tmp+k, arr+right_low, (right_high-right_low+1)*sizeof(int));
		for (i = right_low; i <= right_high; i++)
			tmp[k++] = arr[i];
	}

	for (i = 0; i<high - low + 1; i++)
		arr[low + i] = tmp[i];
	free(tmp);
	return;
}

void merge_sort(int arr[], unsigned int first, unsigned int last)
{

	int mid = 0;
	
	if (first<last)
	{
	
		mid = (first + last) / 2;
		
		/*mid = first/2 + last/2;*/
		//mid = (first & last) + ((first ^ last) >> 1);
		
		merge_sort(arr, first, mid);
		merge_sort(arr, mid + 1, last);
		merge(arr, first, mid, last);
	}
	return;
}

int main()
{

	int i;
	
	int a[N] = { 3，1，4，1，5，9，2，6 };
	
	printf("排序前 \n");
	for (i = 0; i<N; i++)
		printf("%d\t", a[i]);

	merge_sort(a, 0, N - 1);  // 排序

	printf("\n 排序后 \n");
	for (i = 0; i<N; i++)
		printf("%d\t", a[i]); printf("\n");

	system("pause");
	return 0;
}


4. 对下面的 json 字符串 serial 相同的进行去重。

```javascript
[{
	"name": "张三",
		"serial" : "0001"
}, {
	"name": "李四",
	"serial" : "0002"
}, {
	"name": "王五",
	"serial" : "0003"
}, {
	"name": "王五2",
	"serial" : "0003"
}, {
	"name": "赵四",
	"serial" : "0004"
}, {
	"name": "小明",
	"serial" : "005"
}, {
	"name": "小张",
	"serial" : "006"
}, {
	"name": "小李",
	"serial" : "006"
}, {
	"name": "小李2",
	"serial" : "006"
}, {
	"name": "赵四2",
	"serial" : "0004"
}];
	```
	Namelist:[{"name": "张三", "serial" : "0001"}, { "name": "李四", "serial" : "0002" }, { "name": "王五", "serial" : "0003" }, { "name": "王五2", "serial" : "0003" }, { "name": "赵四", "serial" : "0004" }, { "name": "小明", "serial" : "005" }, { "name": "小张", "serial" : "006" }, { "name":"小李", "serial" : "006" }, { "name": "小李2", "serial" : "006" }, { "name": "赵四2", "serial" : "0004" }];

	function UniqueSerial(Namelist)
	{
		var NameArr = [Namelist[0]];
		for (var i = 1; i < Namelist.length; i++)
		{
			var NameItem = Namelist[i];
			var repeat = false;
			for (var j = 0; j < NameArr.length; j++)
			{
				if (NameItem.serial == NameArr[j].serial)
				{
					repeat = true;
					break;
				}
			}
			if (!repeat)
			{
				NameArr.push(NameItem);
			}
		}
		return NameArr;
	}

	5. 把下面给出的扁平化json数据用递归的方式改写成组织树的形式

		```javascript
		[
	{
		"id": "1",
			"name" : "中国",
			"code" : "110",
			"parent" : ""
	},
	{
		"id": "2",
		"name" : "北京市",
		"code" : "110000",
		"parent" : "110"
	},
	{
		"id": "3",
		"name" : "河北省",
		"code" : "130000",
		"parent" : "110"
	},
	{
		"id": "4",
		"name" : "四川省",
		"code" : "510000",
		"parent" : "110"
	},
	{
		"id": "5",
		"name" : "石家庄市",
		"code" : "130001",
		"parent" : "130000"
	},
	{
		"id": "6",
		"name" : "唐山市",
		"code" : "130002",
		"parent" : "130000"
	},
	{
		"id": "7",
		"name" : "邢台市",
		"code" : "130003",
		"parent" : "130000"
	},
	{
		"id": "8",
		"name" : "成都市",
		"code" : "510001",
		"parent" : "510000"
	},
	{
		"id": "9",
		"name" : "简阳市",
		"code" : "510002",
		"parent" : "510000"
	},
	{
		"id": "10",
		"name" : "武侯区",
		"code" : "51000101",
		"parent" : "510001"
	},
	{
		"id": "11",
		"name" : "金牛区",
		"code" : "51000102",
		"parent" : "510001"
	}
		];
	```

		找到树的最底层，一步步加上去。如果从中间层开始加，就需要再深入遍历才能把最底层加上去

		function Tree(s){
			let ind = 0; //判断第一层是不是还有子树
			if (s.length>1)
			{
				for (let i = 0; i<s.length; i++)
				{
					let a = 0;  //计数信号量
					for (let j = i + 1; j<s.length; j++)
					{
						if (s[j].parent == s[i].code)
						{//判断是否有子树
							a++;  //子树计数
							ind++;
						}
					}
					if (a == 0 && s[i].parent != '')
					{ //没有子树，即树的最底层
						for (let n in s)
						{
							//定义children，避免undefined
							s[n].children = s[n].children ? s[n].children : [];
							if (s[n].code == s[i].parent)
							{
								s[n].children.push(s[i]);
							}
						}
						s.splice(i, 1);//删除，该子树已经加入了某项底层
						i--; //删掉子树后后面的数据会填补空缺，退一步才能遍历完全
					}
				}
				if (ind != 0)
				{ //如果还有子树继续遍历第一层
					Tree(s);
				}
			}
			return s;
		}
	function handleTree(s)
	{
		s = Tree(s);
		console.log(s);
		return s;
	}
