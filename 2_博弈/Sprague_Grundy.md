# SG函数

本文转自：[组合游戏 - SG函数和SG定理](http://blog.csdn.net/luomingjun12315/article/details/45555495)


<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在介绍SG函数和SG定理之前我们先介绍介绍必胜点与必败点吧.<br>
</p>
<div class="para"><span style="font-size:14px; color:#ff0000">必胜点和必败点的概念</span><span style="font-size:14px">：</span></div>
<div class="para">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="color:#ff0000">P点</span>：必败点，换而言之，就是谁处于此位置，则在双方操作正确的情况下必败。</div>
<div class="para">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span style="color:#ff0000">N点</span>：必胜点，处于此情况下，双方操作均正确的情况下必胜。</div>
<div class="para"><span style="font-size:14px; color:#ff0000">必胜点和必败点的性质</span><span style="font-size:14px">：</span></div>
<div class="para">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1、所有终结点是 必败点 P 。（我们以此为基本前提进行推理，换句话说，我们以此为假设）</div>
<div class="para">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2、从任何必胜点N 操作，至少有一种方式可以进入必败点 P。</div>
<div class="para">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3、无论如何操作，必败点P 都只能进入 必胜点 N。</div>
<div class="para">我们研究必胜点和必败点的目的时间为题进行简化，有助于我们的分析。通常我们分析必胜点和必败点都是以终结点进行逆序分析。我们以<a target="_blank" target="_blank" href="http://acm.hdu.edu.cn/showproblem.php?pid=1847"><span style="color:#3333ff">hdu 1847 Good Luck in CET-4 Everybody!</span></a>为例：</div>
<div class="para">当 n = 0 时，显然为必败点，因为此时你已经无法进行操作了</div>
<div class="para">当 n = 1 时，因为你一次就可以拿完所有牌，故此时为必胜点</div>
<div class="para">当 n = 2 时，也是一次就可以拿完，故此时为必胜点</div>
<div class="para">当 n = 3 时，要么就是剩一张要么剩两张，无论怎么取对方都将面对必胜点，故这一点为必败点。</div>
<div class="para">以此类推，最后你就可以得到；</div>
<div class="para">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; n&nbsp;&nbsp;&nbsp;&nbsp;：&nbsp;&nbsp; 0&nbsp;&nbsp;&nbsp; 1&nbsp;&nbsp;&nbsp;&nbsp;2&nbsp;&nbsp;&nbsp; 3&nbsp;&nbsp;&nbsp; 4&nbsp;&nbsp; 5&nbsp;&nbsp;&nbsp; 6 ...</div>
<div class="para">position：&nbsp;&nbsp;P&nbsp;&nbsp;&nbsp;&nbsp;N&nbsp;&nbsp; N&nbsp;&nbsp;&nbsp; P&nbsp;&nbsp; N&nbsp;&nbsp; N&nbsp;&nbsp; P ...</div>
<div class="para">你发现了什么没有，对，他们就是成有规律，使用了 P/N来分析，有没有觉得问题变简单了。</div>
<div class="para">现在给你一个稍微复杂一点点的： <a target="_blank" target="_blank" href="http://acm.hdu.edu.cn/showproblem.php?pid=2147">
<span style="color:#3333ff">hdu 2147 kiki's game</span></a></div>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 现在我们就来介绍今天的主角吧。组合游戏的和通常是很复杂的，但是有一种新工具，可以使组合问题变得简单————SG函数和SG定理。</p>
<p><span style="font-size:14px; color:#ff0000">Sprague-Grundy定理（SG定理）：</span></p>
<p><span style="color:#000000">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;<strong>游戏和的SG函数等于各个游戏SG函数的Nim和。</strong>这样就可以将每一个子游戏分而治之，从而简化了问题。而Bouton定理就是Sprague-Grundy定理在Nim游戏中的直接应用，</span>因为单堆的Nim游戏 SG函数满足 SG(x) = x。不知道Nim游戏的请移步：<span style="color:#3366ff"><a target="_blank" target="_blank" href="http://blog.csdn.net/luomingjun12315/article/details/45479073">这里</a></span></p>
<p><span style="font-size:14px; color:#ff0000">SG函数：</span></p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 首先定义mex(minimal excludant)运算，这是施加于一个集合的运算，表示最小的不属于这个集合的非负整数。例如mex{0,1,2,4}=3、mex{2,3,5}=0、mex{}=0。</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 对于任意状态 x ， 定义 SG(x) = mex(S),其中 S 是 x 后继状态的SG函数&#20540;的集合。如 x 有三个后继状态分别为&nbsp;SG(a),SG(b),SG(c)，那么SG(x) = mex{SG(a),SG(b),SG(c)}。&nbsp;这样 集合S 的终态必然是空集，所以SG函数的终态为 SG(x) = 0,当且仅当 x 为必败点P时。</p>
<p><span style="font-size:12px"><strong>【实例】取石子问题</strong></span></p>
<p>有1堆n个的石子，每次只能取{ 1, 3, 4 }个石子，先取完石子者胜利，那么各个数的SG&#20540;为多少？</p>
<p>SG[0]=0，f[]={1,3,4},</p>
<p>x=1 时，可以取走1 - f{1}个石子，剩余{0}个，所以 SG[1] = mex{ SG[0] }= mex{0} = 1;</p>
<p>x=2 时，可以取走2 - f{1}个石子，剩余{1}个，所以 SG[2] = mex{ SG[1] }= mex{1} = 0;</p>
<p>x=3 时，可以取走3 - f{1,3}个石子，剩余{2,0}个，所以 SG[3] = mex{SG[2],SG[0]} = mex{0,0} =1;</p>
<p>x=4 时，可以取走4- &nbsp;f{1,3,4}个石子，剩余{3,1,0}个，所以 SG[4] = mex{SG[3],SG[1],SG[0]} = mex{1,1,0} = 2;</p>
<p>x=5 时，可以取走5 - f{1,3,4}个石子，剩余{4,2,1}个，所以SG[5] = mex{SG[4],SG[2],SG[1]} =mex{2,0,1} = 3;</p>
<p>以此类推.....</p>
<p>&nbsp;&nbsp; x&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0&nbsp; 1&nbsp; 2&nbsp; 3&nbsp; 4&nbsp; 5&nbsp; 6&nbsp; 7&nbsp; 8....</p>
<p>SG[x]&nbsp;&nbsp;&nbsp;&nbsp;0&nbsp; 1&nbsp; 0&nbsp; 1&nbsp; 2&nbsp; 3&nbsp; 2&nbsp; 0&nbsp; 1....</p>
<p>由上述实例我们就可以得到SG函数&#20540;求解步骤，那么计算1~n的SG函数&#20540;步骤如下：</p>
<p>1、使用 数组f 将 可改变当前状态 的方式记录下来。</p>
<p>2、然后我们使用 另一个数组 将当前状态x 的后继状态标记。</p>
<p>3、最后模拟mex运算，也就是我们在标记&#20540;中 搜索 未被标记&#20540; 的最小&#20540;，将其赋&#20540;给SG(x)。</p>
<p>4、我们不断的重复 2 - 3 的步骤，就完成了 计算1~n 的函数&#20540;。</p>
<p>代码实现如下：</p>
<pre class="cpp" name="code">//f[N]:可改变当前状态的方式，N为方式的种类，f[N]要在getSG之前先预处理
//SG[]:0~n的SG函数值
//S[]:为x后继状态的集合
int f[N],SG[MAXN],S[MAXN];
void  getSG(int n){
    int i,j;
    memset(SG,0,sizeof(SG));
    //因为SG[0]始终等于0，所以i从1开始
    for(i = 1; i &lt;= n; i++){
        //每一次都要将上一状态 的 后继集合 重置
        memset(S,0,sizeof(S));
        for(j = 0; f[j] &lt;= i &amp;&amp; j &lt;= N; j++)
            S[SG[i-f[j]]] = 1;  //将后继状态的SG函数值进行标记
        for(j = 0;; j++) if(!S[j]){   //查询当前后继状态SG值中最小的非零值
            SG[i] = j;
            break;
        }
    }
}</pre>
<p><br>
现在我们来一个实战演练（<span style="color:#3333ff"><a target="_blank" target="_blank" href="http://acm.hdu.edu.cn/showproblem.php?pid=1848">题目链接</a></span>）：</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 只要按照上面的思路，解决这个就是分分钟的问题。</p>
<p>代码如下：</p>
<pre class="cpp" name="code">#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#define MAXN 1000 + 10
#define N 20
int f[N],SG[MAXN],S[MAXN];
void getSG(int n){
    int i,j;
    memset(SG,0,sizeof(SG));
    for(i = 1; i &lt;= n; i++){
        memset(S,0,sizeof(S));
        for(j = 0; f[j] &lt;= i &amp;&amp; j &lt;= N; j++)
            S[SG[i-f[j]]] = 1;
        for(j = 0;;j++) if(!S[j]){
            SG[i] = j;
            break;
        }
    }
}
int main(){
    int n,m,k;
    f[0] = f[1] = 1;
    for(int i = 2; i &lt;= 16; i++)
        f[i] = f[i-1] + f[i-2];
    getSG(1000);
    while(scanf(&quot;%d%d%d&quot;,&amp;m,&amp;n,&amp;k),m||n||k){
        if(SG[n]^SG[m]^SG[k]) printf(&quot;Fibo\n&quot;);
        else printf(&quot;Nacci\n&quot;);
    }
    return 0;
}
</pre>