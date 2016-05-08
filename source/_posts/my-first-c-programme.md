title: My first C++ programme
tags:
  - C++
id: 1299
categories:
  - 杂谈
date: 2015-06-07 17:49:28
---

This is awesome!!! I have nerver been thought I could write a C ++ program alone. Now I can !
<pre class="lang:c++ decode:true " title="A simple C ++">#include &lt;iostream&gt;

int readNumber()
{

    using namespace std;
    int x=0;
    cout &lt;&lt; "please input an intger: ";
    cin &gt;&gt; x;
    return x;

}

int addnumber()
{
    int sum;
    int i = readNumber();
    int j = readNumber();
    sum = i + j ;
    return sum;

}

void writeAnswer()
{
    using namespace std;
    //"This is the answer: "&lt;&lt;
    cout &lt;&lt;  addnumber() &lt;&lt; endl;

}

int main()
{
    //addnumber();
    writeAnswer();
    return 0;

}
</pre>
&nbsp;