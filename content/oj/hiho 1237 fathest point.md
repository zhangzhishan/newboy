---
title: hiho 1237 fathest point
tags: hiho, oj
date: 25 Apr 2017 11:16 PM
---

```
#include <stdio.h>
#include <math.h>
double x0,yo,r;
int main()
{
        while(scanf("%lf%lf%lf",&x0,&yo,&r)>0)
        {
            int maxx=floor(x0+r);
            int minx=ceil(x0-r);
            int ansx=0,ansy=0;
            double dis=0;
            for(int x=minx;x<=maxx;x++)
            {
                int y=floor(yo+sqrt(r*r-(x*1.000-x0)*(x*1.000-x0)));
                if(sqrt((x*1.000-x0)*(x*1.000-x0)+(y*1.000-yo)*(y*1.000-yo))>dis||(sqrt((x*1.000-x0)*(x*1.000-x0)+(y*1.000-yo)*(y*1.000-yo))==dis&&((x>ansx||(x==ansx&&y>ansy)))))
                    ansx=x,ansy=y,dis=sqrt((x*1.000-x0)*(x*1.000-x0)+(y*1.000-yo)*(y*1.000-yo));
                y=ceil(yo-sqrt(r*r-(x*1.000-x0)*(x*1.000-x0)));
                if(sqrt((x*1.000-x0)*(x*1.000-x0)+(y*1.000-yo)*(y*1.000-yo))>dis||(sqrt((x*1.000-x0)*(x*1.000-x0)+(y*1.000-yo)*(y*1.000-yo))==dis&&((x>ansx||(x==ansx&&y>ansy)))))
                    ansx=x,ansy=y,dis=(sqrt((x*1.000-x0)*(x*1.000-x0)+(y*1.000-yo)*(y*1.000-yo)));

            }
            printf("%d %d\n",ansx,ansy);
        }
    }
```