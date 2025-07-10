---
title: 1289  403 Forbidden
tags: hiho, oj
date: 13 May 2017 11:57 PM
---
Description
Little Hi runs a web server. Sometimes he has to deny access from a certain set of malicious IP addresses while his friends are still allow to access his server. To do this he writes N rules in the configuration file which look like:
allow 1.2.3.4/30
deny 1.1.1.1
allow 127.0.0.1
allow 123.234.12.23/3
deny 0.0.0.0/0
Each rule is in the form: allow | deny address or allow | deny address/mask.
When there comes a request, the rules are checked in sequence until the first match is found. If no rule is matched the request will be allowed. Rule and request are matched if the request address is the same as the rule address or they share the same first mask digits when both written as 32bit binary number.
For example IP "1.2.3.4" matches rule "allow 1.2.3.4" because the addresses are the same. And IP "128.127.8.125" matches rule "deny 128.127.4.100/20" because 10000000011111110000010001100100 (128.127.4.100 as binary number) shares the first 20 (mask) digits with 10000000011111110000100001111101 (128.127.8.125 as binary number).
Now comes M access requests. Given their IP addresses, your task is to find out which ones are allowed and which ones are denied.
Input
Line 1: two integers N and M.
Line 2-N+1: one rule on each line.
Line N+2-N+M+1: one IP address on each line.
All addresses are IPv4 addresses(0.0.0.0 - 255.255.255.255). 0 \<= mask \<= 32.
  

For 40% of the data: 1 \<= N, M \<= 1000.
For 100% of the data: 1 \<= N, M \<= 100000.
Output
For each request output "YES" or "NO" according to whether it is allowed.
Sample Input
5 5
allow 1.2.3.4/30
deny 1.1.1.1
allow 127.0.0.1
allow 123.234.12.23/3
deny 0.0.0.0/0
1.2.3.4
1.2.3.5
1.1.1.1
100.100.100.100
219.142.53.100
Sample Output
YES
YES
NO
YES
NO

注意是默认放行，之前在这个地方一直WA，另外就是注意输入的ip也使用long long类型，否则移位会变为负号。
	#include<iostream>
	using namespace std;
	
	const int kind = 2;//字母种类
	typedef long long ll;
	struct Treenode//树的结点结构
	{
	    int index;
	    int status;//这个附加变量在本题中记录遍历到该结点形成的字符串出现的次数，在不同题中可记录不同的内容。
	    Treenode *next[kind];//指向儿子结点
	    Treenode(int flag)//每个结点的初始化
	    {
	        status = flag;
	        for(int i=0;i<kind;i++)
	            next[i]=NULL;
	    }
	};
	
	void insert(Treenode *&root, ll ip, int flag, int mask, int index)//向以root为根结点的树中插入串word
	{
	    Treenode *location=root;
	
	    if(location==NULL) {
	        location = new Treenode(-1);
	        root=location;
	    }
	    for (int i = 31; i >= 31 - (mask - 1); i--) {
	        int branch = ((ip & (1 << i)) >> i);
	        if(!location->next[branch]) {
	            location->next[branch]=new Treenode(-1);//如果不存在，建新结点
	        }
	        location=location->next[branch];
	    }
	    if(location->status == -1) {
	        location->status = flag;
	        location->index = index;
	    }
	}
	
	int search(Treenode *root, ll ip)//查找，与插入类似
	{
	    Treenode *location=root;
	    int ans = 1;
	    int index = 100001;
	    if(location==NULL) return 0;
	
	    for (int i = 31; i >= 0; i--)
	    {
	//        cout << location->status;
	        if (location->status != -1 && location->index < 100001)
	        {
	            ans = location->status;
	        }
	        int k = ((ip&(1 << i))>>i);
	        if (location->next[k] == NULL)
	            break;
	        location = location->next[k];
	    }
	
	
	    return ans;
	}
	int main()
	{
	    int m, n;
	    while (scanf("%d%d", &m, &n) != EOF) {
	        Treenode *root = NULL;
	
	        for (int i = 0; i < m; i++) {
	            char a[10];
	            scanf("%s", a);
	            ll ip1, ip2, ip3, ip4;
	            char b;
	            scanf("%lld.%lld.%lld.%lld%c", &ip1, &ip2, &ip3, &ip4, &b);
	
	            ll ip = (ip1 << 24) + (ip2 << 16) + (ip3 << 8) + ip4;
	            int mask = 32;
	            if (b == '/') {
	                scanf("%d", &mask);
	            }
	
	            if (a[0] == 'a') {
	                insert(root, ip, 1, mask, i);
	            } else {
	                insert(root, ip, 0, mask, i);
	            }
	        }
	        for (int j = 0; j < n; ++j) {
	            ll ip1, ip2, ip3, ip4;
	            scanf("%lld.%lld.%lld.%lld", &ip1, &ip2, &ip3, &ip4);
	            ll ip = (ip1 << 24) + (ip2 << 16) + (ip3 << 8) + ip4;
	            if (search(root, ip)) {
	                cout << "YES" << endl;
	            } else {
	                cout << "NO" << endl;
	            }
	        }
	    }
	
	    return 0;
	}

> [呼呼呼山]()(http://code4fun.me)
> 13 May 2017 11:57 PM 

