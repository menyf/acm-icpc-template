# 二维计算几何-wlhz

用前先膜wlhz学长

version 2016.10.10

```
#include<cstdio>
#include<cstdlib>
#include<cstring>
#include<cmath>
#include<ctime>
#include<cassert>
#include<climits>
#include<iostream>
#include<algorithm>
#include<string>
#include<vector>
#include<deque>
#include<list>
#include<set>
#include<map>
#include<stack>
#include<queue>
#include<numeric>
#include<iomanip>
#include<bitset>
#include<sstream>
#include<fstream>
#define scnaf scanf
#define cahr char
#define bug puts("================================");
#define pi (acos(-1.0))
#define eps (1e-8)
#define inf (1<<30)
#define INF (1ll<<62)
using namespace std;
typedef long long ll;
/*====================================================
此几何模板以交大模板为基础 by wlhz
====================================================*/
inline double sqr(const double &x){ return x * x; }
inline int sgn(const double &x){ return x < -eps ? -1 : x > eps; }
struct point { //点 向量
    double x, y;
    point(const double &x = 0, const double &y = 0): x(x), y(y) {}
    friend point operator +  (const point  &a, const point  &b){ return point(a.x + b.x, a.y + b.y); }
    friend point operator -  (const point  &a, const point  &b) { return point(a.x - b.x, a.y - b.y); }
    friend point operator *  (const point  &a, const double &b) { return point(a.x * b, a.y * b); }
    friend point operator *  (const double &a, const point  &b) { return point(a * b.x, a * b.y); }
    friend point operator /  (const point  &a, const double &b) { return point(a.x / b, a.y / b); }
    friend bool  operator == (const point  &a, const point  &b) { return !sgn(a.x - b.x) && !sgn(a.y - b.y); }
    friend bool  operator != (const point  &a, const point  &b) { return !(a==b); }
    friend bool  operator <  (const point  &a, const point  &b) { return sgn(a.x - b.x) < 0 || (sgn(a.x - b.x) == 0 && sgn(a.y - b.y) < 0); }
    ///friend bool  operator <  (const point  &a, const point  &b) { return a.x<b.x || (a.x == b.x && a.y<b.y); }//比较原始输入数据不加eps
    double norm(){ return sqrt(sqr(x) + sqr(y)); }                                                                      //向量的模
    friend double det(const point &a, const point &b){ return a.x * b.y - a.y * b.x; }                                  //2个向量的叉积
    friend double xmult(const point &p1,const point &p2,const point &p0){ return det(p1-p0,p2-p0); }                    //3个点的叉积
    friend double dot(const point &a, const point &b){ return a.x * b.x + a.y * b.y; }                                  //2个向量的点积
    friend double dis(const point &a, const point &b){ return (a - b).norm(); }                                         //两点之间距离
    friend point intersection(point u1,point u2,point v1,point v2){return u1+det(u1-v1,v1-v2)/det(u1-u2,v1-v2)*(u2-u1);}//两个线段的交点
    double arg(){ return atan2(y, x); }                                                                                 //返回弧度
    ///{double res = atan2(y, x); if(res < -pi / 2 + eps) res += 2 * pi; return res;}                                      修正精度的版本
    friend double arg_2(point u,point v){  return acos( dot(u,v) / (u.norm() * v.norm()) ); }                           //两向量之间的弧度 by wlhz
    friend double arg_3(const point &a,const point &b,const point &c){ return arg_2(a-b,c-b);}                                               //求角abc的弧度 by wlhz
    point rotate(const double &angle){ return rotate(cos(angle), sin(angle)); }                                         //向量逆时针旋转angle弧度
    point rotate(const point &p, const double &angle){ return (*this-p).rotate(angle)+p; }                              //向量绕点逆时针旋转angle弧度
    point rotate(const double &cosa, const double &sina){ return point(x * cosa - y * sina, x * sina + y * cosa); }
    int in()  { return ~scanf("%lf%lf", &x, &y); }
    void out(){ printf("%.2f %.2f\n", x, y); }
};

struct line{ //线段 直线 射线
    point s,t;
    line(const point &s = point(), const point &t = point()): s(s), t(t) {}
    point vec() const{ return t - s; }//化成矢量
    double norm() const{ return vec().norm(); } //长度
    bool is_point_line(const point &p) const { return sgn(det(p - s, t - s)) == 0; }                       //点在直线上
    bool is_point_seg(const point &p) const { return is_point_line(p) && sgn(dot(p - s, p - t)) <= 0; }    //点在线段上
    bool is_point_seg_a(const point &p) { return is_point_line(p) && sgn(dot(p - s, p - t)) < 0; }         //点在线段上不含端点
    point pedal_point_line(const point &p){ return s + vec() * ((dot(p - s, vec()) / norm()) / (norm()));} //点到直线的垂足
    double dis_point_line(const point &p) { return fabs(det(p - s, vec()) / norm()); }                     //点到直线的距离
    //点到线段的距离
    double dis_point_seg(const point &p){
        if (sgn(dot(p - s, t - s)) < 0) return (p - s).norm();
        if (sgn(dot(p - t, s - t)) < 0) return (p - t).norm();
        return dis_point_line(p);
    }
    // 计算点p与直线L的相对关系 ,返回ONline,LEFT,RIGHT 左0 上1 右2
    int relation(const point &p){
        double res =det(t-s, p-s);
        return res < -eps ? 2 : res < eps;
    }
    // 判断点p1, p2是否在直线L的同侧或者同时在直线上
    bool sameside(const point &p1,const point &p2){ return relation(p1) == relation(p2); }
    // 求二维平面上点p关于直线L的对称点
    point sympoint(point p){ return 2 * s - p + 2 * (t-s) * dot(p-s,t-s)/(sqr(t.x-s.x)+sqr(t.y-s.y)); }
    //判断两直线是否平行
    friend bool parallel(const line &l1, const line &l2){ return !sgn(det(l1.vec(), l2.vec())); }
    //两直线的交点
    friend point intersection_line_line(const line &l1, const line &l2) {//利用相似三角形对应边成比例
        double s1 = det(l1.s - l2.s, l2.vec());
        double s2 = det(l1.t - l2.s, l2.vec());
        return (l1.t * s1 - l1.s * s2) / (s1 - s2);
    }
    //两直线的交点 另一种方法
    friend point intersection_line_line_2(const line &u,const line &v){ return u.s+(u.t-u.s)*det(u.s-v.s,v.s-v.t)/det(u.s-u.t,v.s-v.t); }
    //判断线段交
    friend bool is_seg_seg(line l1,line l2){
        if(!sgn(det(l2.s - l1.s, l1.vec())) && !sgn(det(l2.t - l1.s, l1.vec()))){
        return l1.is_point_seg(l2.s) || l1.is_point_seg(l2.t) || l2.is_point_seg(l1.s) || l2.is_point_seg(l1.t);
        }
        return !l1.sameside(l2.s, l2.t) && !l2.sameside(l1.s, l1.t);
    }
    //直线沿法线方向移动d距离
    friend line move(const line &l, const double &d){
        point t = l.vec(); t = t / t.norm(); t = t.rotate(pi / 2);
        return line(l.s + t * d, l.t + t * d);
    }
    // 计算线段L1到线段L2的最短距离
    friend double dis_seg_seg(line &L1, line &L2){
        double d1, d2, d3, d4;
        if (is_seg_seg(L1, L2))
            return 0;
        else{
            d1 = dis( L2.dis_point_seg(L1.s) , L1.s );
            d2 = dis( L2.dis_point_seg(L1.t) , L1.t );
            d3 = dis( L1.dis_point_seg(L2.s) , L2.s );
            d4 = dis( L1.dis_point_seg(L2.t) , L2.t );
            return min( min(d1, d2), min(d3, d4) );
        }
    }
    // 求两直线的夹角，返回值是0~Pi之间的弧度
    friend double arg_line_line(line L1,line L2) {
        point u = L1.t - L1.s;    point v = L2.t - L2.s;
        return acos( dot(u,v) / (u.norm() * v.norm()) );
    }
    bool in(){ return s.in()&&t.in(); }
    void out(){ s.out(); t.out(); }
};

struct triangle//三角形
{
    point a,b,c;
    //外心
    point circumcenter(){
        line u,v;
        u.s=(a+b)/2;
        u.t.x=u.s.x-a.y+b.y;
        u.t.y=u.s.y+a.x-b.x;
        v.s=(a+c)/2;
        v.t.x=v.s.x-a.y+c.y;
        v.t.y=v.s.y+a.x-c.x;
        return intersection_line_line(u,v);
    }
    //内心
    point incenter(){
        line u,v;
        u.s=a;
        double m=atan2(b.y-a.y,b.x-a.x);
        double n=atan2(c.y-a.y,c.x-a.x);
        u.t.x=u.s.x+cos((m+n)/2);
        u.t.y=u.s.y+sin((m+n)/2);
        v.s=b;
        m=atan2(a.y-b.y,a.x-b.x);
        n=atan2(c.y-b.y,c.x-b.x);
        v.t.x=v.s.x+cos((m+n)/2);
        v.t.y=v.s.y+sin((m+n)/2);
        return intersection_line_line(u,v);
    }
    //垂心
    point perpencenter(){
        line u,v;
        u.s=c;
        u.t.x=u.s.x-a.y+b.y;
        u.t.y=u.s.y+a.x-b.x;
        v.s=b;
        v.t.x=v.s.x-a.y+c.y;
        v.t.y=v.s.y+a.x-c.x;
        return intersection_line_line(u,v);
    }

    //重心
    //到三角形三顶点距离的平方和最小的点
    //三角形内到三边距离之积最大的点
    point barycenter(){
        line u((a+b)/2,c),v((a+c)/2,b);
        return intersection_line_line(u,v);
    }

    //费马点 到三角形三顶点距离之和最小的点 by wlhz
    point fermentpoint_wlhz(){
      if(arg_3(a,b,c)>=2*pi/3) return b;
      if(arg_3(b,a,c)>=2*pi/3) return a;
      if(arg_3(a,c,b)>=2*pi/3) return c;
      point ab=(a+b)/2,ac=(a+c)/2;
      point z1=sqrt(3)*(a-ab),z2=sqrt(3)*(a-ac);
      z1.rotate(pi/2);
      z2.rotate(pi/2);
      if(arg_2(z1,c-ab)<pi/2){
          z1.x=-z1.x; z1.y=-z1.y;
      }
      if(arg_2(z2,b-ac)<pi/2){
          z2.x=-z2.x; z2.y=-z2.y;
      }
      return intersection(c,ab+z1,b,ac+z2);

    }
    // 模拟退火求费马点
    point fermentpoint(){
        point u,v;
        double step=fabs(a.x)+fabs(a.y)+fabs(b.x)+fabs(b.y)+fabs(c.x)+fabs(c.y);
        u=(a+b+c)/3;
        while (step>1e-10)
            for (int k=0; k<10; step/=2,k++)
            for (int i=-1; i<=1; i++)
            for (int j=-1; j<=1; j++){
            v.x=u.x+step*i;
            v.y=u.y+step*j;
            if (dis(u,a)+dis(u,b)+dis(u,c)>dis(v,a)+dis(v,b)+dis(v,c)) u=v;
            }
        return u;
    }
};
struct polygon//多边形
{
    #define next(i) ((i+1)%n)
    int n;
    vector<point> p;
    polygon(int n = 0): n(n){ p.resize(n); }
    //polygon(vector<point> &v):p(v){}
    //多边形周长
    double perimeter() {double sum=0;for(int i=0;i<n;i++) sum+=(p[next(i)]-p[i]).norm();return sum;}
    //多边形面积
    double area(){ double sum=0;for(int i=0;i<n;i++) sum+=det(p[i],p[next(i)]);return fabs(sum)/2;} //eps？
    //判断点与多边形的位置关系 0外, 1内,2边上
    int pointin(const point &t){
        int num = 0;
        for (int i = 0; i < n; i++){
            if (line(p[i], p[next(i)]).is_point_seg(t)) return 2;
            int k = sgn(det(p[next(i)] - p[i], t - p[i]));
            int d1 = sgn(p[i].y - t.y);
            int d2 = sgn(p[next(i)].y - t.y);
            if (k > 0 && d1 <= 0 && d2 > 0) num++;
            if (k < 0 && d2 <= 0 && d1 > 0) num--;
        }
        return num % 2;
    }
    //多边形重心
    point masscenter(){
        point ans;
        if (sgn(area()) == 0) return ans;
        for (int i = 0; i < n; i++)
            ans = ans + (p[i] + p[next(i)]) * det(p[i], p[next(i)]);
        return ans / area() / 6 + eps; //要加eps吗？
    }
    //判断多边形是否为凸多边形 (需要已经排好序)
    bool is_convex(){//不允许3点共线
        int s[3]= {1,1,1};
        for (int i=0; i<n&&(s[0]||s[2])&&s[1]; i++)
            s[1 + sgn(xmult(p[next(i)] ,p[next(next(i))] ,p[i]))]=0;
        return (s[0]||s[2])&&s[1];
    }
    bool is_convex_3(){//允许3点共线
        int s[3]= {1,1,1};
        for (int i=0; i<n&&(s[0]||s[2]); i++)
            s[1 + sgn(xmult(p[next(i)] ,p[next(next(i))] ,p[i]))]=0;
        return (s[0]||s[2]);
    }
    //多边形边界上格点的数量
    ll borderpointnum(){
        ll num = 0;
        for (int i = 0; i < n; i++)
        num+= __gcd((ll)fabs(p[next(i)].x-p[i].x),(ll)fabs(p[next(i)].y-p[i].y));
        return num;
    }
    //多边形内格点数量
    ll insidepointnum(){ return ll(area()) + 1 - borderpointnum() / 2;}
    inline int dot_online_in(point p,point l1,point l2){
    return sgn(xmult(p,l1,l2)) && (l1.x-p.x)*(l2.x-p.x)<eps && (l1.y-p.y)*(l2.y-p.y) <eps;
    }
    //判线段在任意多边形内,顶点按顺时针或逆时针给出,与边界相交返回1
    int inside_polygon(line l){
        vector<point>t;
        point tt,l1=l.s,l2=l.t;
        if (!pointin(l.s)||!pointin(l.t)) return 0;
        for (int i=0;i<n;i++)
             if (l.sameside(p[i],p[(i+1)%n])&&l.sameside(p[i],p[(i+1)%n]))  return 0;
        else if (dot_online_in(l1,p[i],p[(i+1)%n])) t.push_back(l1);
        else if (dot_online_in(l2,p[i],p[(i+1)%n])) t.push_back(l2);
        else if (dot_online_in(p[i],l1,l2)) t.push_back(p[i]);
        for (int i=0;i<t.size();i++)
            for (int j=i+1;j<t.size();j++)
                if (!pointin((t[i]+t[j])/2)) return 0;
        return 1;
    }
    void in() {
        scnaf("%d",&n);
         p.clear(); point pp;
         for(int i=0;i<n;i++){
                pp.in();
                p.push_back(pp);
        }
    }
    void out(){ for (int i = 0; i < n; i++) p[i].out(); }
};

struct convex : public polygon //凸包
{
    convex(int n = 0): polygon(n) {}
    //convex(vector<point> &v):polygon(v){}

    //需要先求凸包，若凸包每条边除端点外都有点，则可唯一确定凸包
     bool isunique(vector<point> &v){
        if (sgn(area()) == 0) return 0;
        for (int i = 0; i < n; i++){
            line l(p[i], p[next(i)]);
            bool flag = 0;
            for (int j = 0; j < v.size(); j++)
                if (l.is_point_seg_a(v[j])){
                    flag = 1;
                    break;
                }
            if (!flag) return 0;
        }
        return 1;
    }
    //O(n)时间内判断点是否在凸包内 包含边
    bool containon(const point &a){
        for (int sign=0,i = 0; i < n; i++){
            int x = sgn(det(p[i] - a, p[next(i)] - a));
            if (x==0) continue;//return 0;//改成不包含边
            if (!sign) sign=x;
            else if(sign!=x) return 0;
        }
        return 1;
    }
    //O(logn)时间内判断点是否在凸包内
    bool containologn(const point &a){
        point g = (p[0] + p[n / 3] + p[2 * n / 3]) / 3.0;
        int l = 0, r = n;
        while(l + 1 < r){
            int m = (l + r) / 2;
            if (sgn(det(p[l] - g, p[m] - g)) > 0){
                if (sgn(det(p[l] - g, a - g)) >= 0 && sgn(det(p[m] - g, a - g)) < 0) r = m;
                else l = m;
            }
            else{
                if (sgn(det(p[l] - g, a - g)) < 0 && sgn(det(p[m] - g, a - g)) >= 0) l = m;
                else r = m;
            }
        }
        return sgn(det(p[r % n] - a, p[l] - a)) - 1;
    }
    //最远点对（直径）
    int first, second; //最远的两个点对应标号
    double diameter(){
        double mx = 0;
        if (n == 1){
            first = second = 0;
            return mx;
        }
        for (int i = 0, j = 1; i < n; i++){
            while(sgn(det(p[next(i)] - p[i], p[j] - p[i]) -
                      det(p[next(i)] - p[i], p[next(j)] - p[i])) < 0)
                j = next(j);
            double d = dis(p[i], p[j]);
            if (d > mx){
                mx = d;
                first = i;
                second = j;
            }
            d = dis(p[next(i)], p[next(j)]);
            if (d > mx){
                mx = d;
                first = next(i);
                second = next(j);
            }
        }
        return mx;
    }

    //凸包是否与直线有交点O(log(n)), 需要On的预处理, 适合判断与直线集是否有交点
    ///poj1912没过
    vector<double> ang; //角度
    bool isinitangle;
    int finda(const double &x){ return upper_bound(ang.begin(), ang.end(), x) - ang.begin();}
    double getangle(const point &p) { //获取向量角度[0, 2pi]
        double res = atan2(p.y, p.x);   //(-pi, pi]
        if (res < 0) res += 2 * pi; //为何不可以
       // if(res < -pi / 2 + eps) res += 2 * pi; //eps修正精度
        return res;
    }
    void initangle(){
        for(int i = 0; i < n; i++)
            ang.push_back(getangle(p[next(i)] - p[i]));
        isinitangle = 1;
    }
    bool isxline(const line &l){//
        if(!isinitangle) initangle();
        int i = finda(getangle(l.t - l.s));
        int j = finda(getangle(l.s - l.t));
        if(sgn(det(l.t - l.s, p[i] - l.s) * det(l.t - l.s, p[j] - l.s) >= 0))
            ///if(sgn(det(l.t - l.s, p[i] - l.s)>=0);
            return 0;
        return 1;
    }
};
convex convexhull(vector<point> &a){ //从一个vector获取凸包
    convex res(2 * a.size() + 5); //为何？经测试好像只需要a.size()？
    sort(a.begin(), a.end());
    a.erase(unique(a.begin(), a.end()), a.end());//去重点
    int m = 0;
    for (int i = 0; i < a.size(); i++){
        //<=0则允许3点共线，<0则不允许
        while(m > 1 && sgn(det(res.p[m - 1] - res.p[m - 2], a[i] - res.p[m - 2])) <= 0) m--;
        res.p[m++] = a[i];
    }
    int k = m;
    for (int i = a.size() - 2; i >= 0; i--){
        while(m > k && sgn(det(res.p[m - 1] - res.p[m - 2], a[i] - res.p[m - 2])) <= 0) m--;
        res.p[m++] = a[i];
    }
    if (m > 1) m--;
    res.p.resize(m);
    res.n = m;
    return res;
}

struct halfplane: public line {//半平面
    //ax+by+c<=0
    double a, b, c;
    //s->t的左侧表示半平面
    halfplane(const point &s = point(), const point &t = point()): line(s, t)
    {
        a = t.y - s.y;
        b = s.x - t.x;
        c = det(t, s);
    }
    halfplane(double a, double b, double c): a(a), b(b), c(c) {}
    //求点p带入直线方程的值
    double calc(const point &p) const { return p.x * a + p.y * b + c; }
    //好像跟intersection_line_line一样，那个是4个点计算。这个是用abc与两点进行计算
    friend point halfxline(const halfplane &h, const line &l){
        point res;
        double t1 = h.calc(l.s), t2 = h.calc(l.t);
        res.x = (t2 * l.s.x - t1 * l.t.x) / (t2 - t1);
        res.y = (t2 * l.s.y - t1 * l.t.y) / (t2 - t1);
        return res;
    }
    //用abc进行计算 尚未测试
    friend point halfxhalf(const halfplane &h1, const halfplane&h2){
        return point((h1.b * h2.c - h1.c * h2.b) / (h1.a * h2.b - h2.a * h1.b) + eps,
                     (h1.a * h2.c - h2.a * h1.c) / (h1.b * h2.a - h1.a * h2.b) + eps);
    }
    //凸多边形与半平面交(cut)
    friend convex halfxconvex(const halfplane &h, const convex &c){
        convex res;
        for (int i = 0; i < c.n; i++){
            if (h.calc(c.p[i]) < -eps)
                res.p.push_back(c.p[i]);
            else{
                int j = i - 1;
                if (j < 0) j = c.n - 1;
                if (h.calc(c.p[j]) < -eps)
                    res.p.push_back(halfxline(h, line(c.p[j], c.p[i])));
                j = i + 1;
                if (j == c.n) j = 0;
                if (h.calc(c.p[j]) < -eps)
                    res.p.push_back(halfxline(h, line(c.p[i], c.p[j])));
            }
        }
        res.n = res.p.size();
        return res;
    }
    //点在半平面内
    friend int satisfy(const point &p, const halfplane &h){ return sgn(det(p - h.s, h.t - h.s)) <= 0; }
    friend bool operator <(const halfplane &h1, const halfplane &h2){
        int res = sgn(h1.vec().arg() - h2.vec().arg());
        return res == 0 ? satisfy(h1.s, h2) : res < 0;
    }
    //半平面交出的凸多边形
    friend convex halfx(vector<halfplane> &v){
        sort(v.begin(), v.end());
        deque<halfplane> q;
        deque<point> ans;
        q.push_back(v[0]);
        for (int i = 1; i < v.size(); i++){
            if (sgn(v[i].vec().arg() - v[i - 1].vec().arg()) == 0)
                continue;
            while(ans.size() > 0 && !satisfy(ans.back(), v[i])){
                ans.pop_back(); q.pop_back();
            }
            while(ans.size() > 0 && !satisfy(ans.front(), v[i])){
                ans.pop_front(); q.pop_front();
            }
            ans.push_back(intersection_line_line(q.back(), v[i]));
            q.push_back(v[i]);
        }
        while(ans.size() > 0 && !satisfy(ans.back(), q.front())){
            ans.pop_back(); q.pop_back();
        }
        while(ans.size() > 0 && !satisfy(ans.front(), q.back())){
            ans.pop_front(); q.pop_front();
        }
        ans.push_back(intersection_line_line(q.back(), q.front()));
        convex c(ans.size());
        int i = 0;
        for (deque<point>::iterator it = ans.begin(); it != ans.end(); it++, i++)
            c.p[i] = *it;
        return c;
    }
};
//多边形的核，逆时针
convex core(const polygon &a)
{
    convex res;
    res.p.push_back(point(-inf, -inf));
    res.p.push_back(point(inf, -inf));
    res.p.push_back(point(inf, inf));
    res.p.push_back(point(-inf, inf));
    res.n = 4;
    for (int i = 0; i < a.n; i++)
        res = halfxconvex(halfplane(a.p[i], a.p[(i + 1) % a.n]), res);
    return res;
}
//凸多边形交出的凸多边形
convex convexxconvex(convex &c1, convex &c2){
    vector<halfplane> h;
    for (int i = 0; i < c1.p.size(); i++)
        h.push_back(halfplane(c1.p[i], c1.p[(i + 1) % c1.p.size()]));
    for (int i = 0; i < c2.p.size(); i++)
        h.push_back(halfplane(c2.p[i], c2.p[(i + 1) % c2.p.size()]));
    return halfx(h);
}
double mysqrt(double n) { return sqrt(max(0.0, n)); } //防止出现sqrt(-eps)的情况
struct circle//圆
{
    point o;
    double r;
    circle(point o = point(), double r = 0): o(o), r(r) {}
    bool operator ==(const circle &c){ return o == c.o && !sgn(r - c.r); }
    double area(){ return pi * r * r; }
    double length(){ return r * pi * 2; }
    //点在圆内，不包含边界
    bool pointin(const point &p){ return sgn((p - o).norm() - r) < 0; }
    //点在圆内，包含边界
    bool pointin_(const point &p){ return sgn((p - o).norm() - r) <= 0; }
    //判直线和圆相交,包括相切
    friend int intersect_line_circle(line L, circle c){ return L.dis_point_line(c.o)<c.r+eps; }
    //判线段和圆相交,包括端点和相切
    friend int intersect_seg_circle(line L, circle c){
        double t1=dis(c.o,L.s)-c.r,t2=dis(c.o,L.t)-c.r;
        point t=c.o;
        if (t1<eps||t2<eps)
            return t1>-eps||t2>-eps;
        t.x+=L.s.y-L.t.y;
        t.y+=L.t.x-L.s.x;
        return xmult(L.s,c.o,t)*xmult(L.t,c.o,t)<eps&&L.dis_point_line(c.o)<c.r+eps;
    }
    //判圆和圆相交,包括相切
    friend int intersect_circle_circle(circle c1, circle c2){
        return dis(c1.o,c2.o)<c1.r+c2.r+eps&&dis(c1.o,c2.o)>fabs(c1.r-c2.r)-eps;
    }
    //计算圆上到点p最近点,如p与圆心重合,返回p本身
    friend point dot_point_circle(point p,circle C){
        point u,v,c=C.o;
        if (dis(p,c)<eps)
            return p;
        u.x=c.x+C.r*fabs(c.x-p.x)/dis(c,p);
        u.y=c.y+C.r*fabs(c.y-p.y)/dis(c,p)*((c.x-p.x)*(c.y-p.y)<0?-1:1);
        v.x=c.x-C.r*fabs(c.x-p.x)/dis(c,p);
        v.y=c.y-C.r*fabs(c.y-p.y)/dis(c,p)*((c.x-p.x)*(c.y-p.y)<0?-1:1);
        return dis(u,p)<dis(v,p)?u:v;
    }
    //圆与线段交 用参数方程表示直线：P=A+t*(B-A)，带入圆的方程求解t
    friend vector<point> intersection_seg_circle(const line &l,const circle &c)
    {
        double dx = l.t.x - l.s.x, dy = l.t.y - l.s.y;
        double A = dx * dx + dy * dy;
        double B = 2 * dx * (l.s.x - c.o.x) + 2 * dy * (l.s.y - c.o.y);
        double C = sqr(l.s.x - c.o.x) + sqr(l.s.y - c.o.y) - sqr(c.r);
        double delta = B * B - 4 * A * C;
        vector<point> res;
        if(A<eps) return res;
        if(sgn(delta) >= 0) //or delta > -eps ?
        {
            //可能需要注意delta接近-eps的情况，所以使用mysqrt
            double w1 = (-B - mysqrt(delta)) / (2 * A);
            double w2 = (-B + mysqrt(delta)) / (2 * A);
            if(sgn(w1 - 1) <= 0 && sgn(w1) >= 0)
                res.push_back(l.s + w1 * (l.t - l.s));
            if(sgn(w2 - 1) <= 0 && sgn(w2) >= 0 && fabs(w1-w2)>eps)
                res.push_back(l.s + w2 * (l.t - l.s));
        }
        return res;
    }
    //圆与直线交
    friend vector<point> intersection_line_circle(const line &l,const circle &c)
    {
        double dx = l.t.x - l.s.x, dy = l.t.y - l.s.y;
        double A = dx * dx + dy * dy;
        double B = 2 * dx * (l.s.x - c.o.x) + 2 * dy * (l.s.y - c.o.y);
        double C = sqr(l.s.x - c.o.x) + sqr(l.s.y - c.o.y) - sqr(c.r);
        double delta = B * B - 4 * A * C;
        vector<point> res;
        if(A<eps) return res;
        if(sgn(delta) >= 0) //or delta > -eps ?
        {
            double w1 = (-B - mysqrt(delta)) / (2 * A);
            double w2 = (-B + mysqrt(delta)) / (2 * A);
            res.push_back(l.s + w1 * (l.t - l.s));
            if(fabs(w1-w2)>eps)
            res.push_back(l.s + w2 * (l.t - l.s));
        }
        return res;
    }
    //计算圆与圆的交点 保证圆不重合
    friend vector<point> intersection_circle_circle(circle a,circle b){
        point c1=a.o,c2=b.o;
        double r1=a.r,r2=b.r;
        vector<point> vec;
        if(dis(a.o,b.o)+eps>a.r+b.r&&dis(a.o,b.o)<fabs(a.r-b.r)+eps) return vec;
        line L;
        double t=(1 + (sqr(a.r)-sqr(b.r))/sqr(dis(a.o,b.o))) / 2;
        L.s=c1+(b.o-a.o)*t;
        L.t.x=L.s.x+a.o.y-b.o.y;
        L.t.y=L.s.y-a.o.x+b.o.x;
        return intersection_line_circle(L,a);
    }
    //将向量p逆时针旋转angle角度
    //求圆外一点对圆(o,r)的切点
    friend vector<point> Tangent_point_circle(point poi,circle C){
        point o=C.o; double r=C.r;
        vector<point>vec;
        double dis=(poi-o).norm();
        if(dis<r-eps) return vec;
        if(fabs(dis-r)<eps) {
            vec.push_back(poi); return vec;
        }
        point result1,result2;
        double line=sqrt((poi.x-o.x)*(poi.x-o.x)+(poi.y-o.y)*(poi.y-o.y));
        double angle=acos(r/line);
        point unitvector,lin;
        lin.x=poi.x-o.x;
        lin.y=poi.y-o.y;
        unitvector.x=lin.x/sqrt(lin.x*lin.x+lin.y*lin.y)*r;
        unitvector.y=lin.y/sqrt(lin.x*lin.x+lin.y*lin.y)*r;
        result1=unitvector.rotate(-angle)+o;
        result2=unitvector.rotate(angle)+o;
        vec.push_back(result1);
        vec.push_back(result2);
        return vec;
    }
    //扇形面积 a->b
    double sectorarea(const point &a, const point &b) const {
        double theta = atan2(a.y, a.x) - atan2(b.y, b.x);
        while (theta < 0) theta += 2 * pi;
        while (theta > 2 * pi) theta -= 2 * pi;
        theta = min(theta, 2 * pi - theta);
        return theta * r * r / 2.0;
    }
    //与线段AB的交点计算面积 a->b
    double area_seg_circle(point a,point b) const {
        vector<point> p = intersection_seg_circle(line(a, b),*this);
        bool ina = sgn((a-o).norm() - r) < 0;
        bool inb = sgn((b-o).norm() - r) < 0;
        a=a-o; b=b-o;
        if (ina){
            if (inb) return fabs(det(a, b)) / 2;
            else return fabs(det(a,p[0]))/2+sectorarea(p[0],b);
        }
        else{
            if (inb) return fabs(det(p[0],b))/2+sectorarea(a,p[0]);
            else{
                if (p.size() == 2)
                    return sectorarea(a,p[0])+sectorarea(p[1],b)+fabs(det(p[0],p[1]))/2;
                else
                    return sectorarea(a,b);
            }
        }
    }
    //圆与多边形面积交，结果可以尝试+eps
    friend double area_polygon_circle( const polygon &a, const circle &c){
        int n = a.p.size();
        double ans = 0;
        for(int i = 0; i < n; i++)
        {
            int gg=sgn(det(a.p[i] - c.o, a.p[next(i)] - c.o));
            if(gg!=0) ans+=c.area_seg_circle(a.p[i],a.p[next(i)]) * gg;
        }
        return fabs(ans);
    }
    //两个圆的公共面积 by wlhz
    friend double area_circle_circle(const circle & A, const circle & B){
        double ans = 0.0;
        circle M = (A.r > B.r) ? A : B;
        circle N = (A.r > B.r) ? B : A;
        double D = dis(M.o, N.o);
        if ((D < M.r + N.r) && (D > M.r - N.r)){
            double alpha = 2.0 * acos((M.r * M.r + D * D - N.r * N.r) / (2.0 * M.r * D));
            double beta  = 2.0 * acos((N.r * N.r + D * D - M.r * M.r) / (2.0 * N.r * D));
            ans = (alpha/(2*pi))*M.area() + (beta/(2*pi))*N.area() - 0.5*M.r*M.r*sin(alpha) - 0.5*N.r*N.r*sin(beta);
        }
        else if (D <= M.r - N.r)
            ans = N.area();
        return ans;
    }

    //三点求圆
    circle getcircle3(const point &p0, const point &p1, const point &p2)
    {
        double a1 = p1.x - p0.x, b1 = p1.y - p0.y, c1 = (a1 * a1 + b1 * b1) / 2;
        double a2 = p2.x - p0.x, b2 = p2.y - p0.y, c2 = (a2 * a2 + b2 * b2) / 2;
        double d = a1 * b2 - a2 * b1;
        point o(p0.x + (c1 * b2 - c2 * b1) / d, p0.y + (a1 * c2 - a2 * c1) / d);
        return circle(o, (o - p0).norm());
    }
    //直径上两点求圆
    circle getcircle2(const point &p0, const point &p1)
    {
        point o((p0.x + p1.x) / 2, (p0.y + p1.y) / 2);
        return circle(o, (o - p0).norm());
    }
    //最小圆覆盖 用之前可以随机化random_shuffle
    circle mincircover(vector<point> &a)
    {
        int n = a.size();
        circle c(a[0], 0);
        for (int i = 1; i < n; i++)
            if (!c.pointin(a[i]))
            {
                c.o = a[i];
                c.r = 0;
                for (int j = 0; j < i; j++)
                    if (!c.pointin(a[j]))
                    {
                        c = getcircle2(a[i], a[j]);
                        for (int k = 0; k < j; k++)
                            if (!c.pointin(a[k]))
                                c = getcircle3(a[i], a[j], a[k]);
                    }
            }
        return c;
    }
     //线段在圆内的长度
    friend double length_seg_circle(point p , point v , circle C) {
    double a = v.x , b = p.x - C.o.x , c = v.y , d = p.y - C.o.y;
    double e = a * a + c * c , f = 2 * (a * b + c * d) , g = b * b + d * d - C.r * C.r;
    double delta = f * f - 4 * e * g;
    if (sgn(delta) <= 0) return 0;
    double t1 = (-f - sqrt(delta)) / (e + e);
    double t2 = (-f + sqrt(delta)) / (e + e);
    vector<double> V; V.push_back(t1); V.push_back(t2);
    V.push_back(0);  V.push_back(1);
    sort(V.begin() , V.end()); double res = 0;
    for (int i = 0 ; i + 1 < V.size() ; ++ i) {
        double w = (V[i] + V[i + 1]) * 0.5;
        if (sgn(w) >= 0 && sgn(w - 1) <= 0 && C.pointin_(p + v * w) )
            res += (v * (V[i + 1] - V[i])).norm();
    }
    return res;
    }
    //圆C2在圆C1内的长度
    friend double getcirclecircleIntersection(circle C1 , circle C2) {
    double d = (C1.o - C2.o).norm();
    if (sgn(d) == 0) {
        if (sgn(C1.r - C2.r) >= 0)
            return 2 * pi * C2.r;
        return 0;
    }
    if (sgn(C1.r - C2.r) >= 0 && sgn(C1.r - C2.r - d) >= 0) return 2 * pi * C2.r;
    if (sgn(C1.r + C2.r - d) < 0) return 0;
    if (sgn(fabs(C1.r - C2.r) - d) > 0) return 0;
    double a = (C1.o - C2.o).arg(); // acos 内可能越界
    double w = (C2.r * C2.r + d * d - C1.r * C1.r) / (2 * C2.r * d);
    double da = acos(max(-1.0 , min(1.0 , w)));
    return C2.r * da * 2;
    }
};

/*===========================模板分割线=======================================*/
```