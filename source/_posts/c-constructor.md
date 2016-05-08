title: c++ constructor
tags:
  - constructor
id: 1325
categories:
  - 杂谈
date: 2015-07-01 23:16:57
---

<pre class="theme:monokai lang:default decode:true  " title="constructor">#include&lt;iostream&gt;

class Date
{
private:
    int m_nDay;
    int m_nMonth;
    int m_nYear;

public:
    Date(int nDay=1, int nMonth=7, int nYear=2015)
    {
        m_nDay = nDay;
        m_nMonth = nMonth;
        m_nYear = nYear;
    }
int GetnDay() {return m_nDay; }
int GetMonth() {return m_nMonth; }
int GetYear() {return m_nYear; }

};

int main()
{
    Date cdate;
    std::cout&lt;&lt;cdate.GetYear() &lt;&lt;"-"&lt;&lt;cdate.GetMonth()&lt;&lt;"-"&lt;&lt;cdate.GetnDay()&lt;&lt;std::endl;
    Date cunix(1,1,1970);
    std::cout&lt;&lt;cunix.GetYear() &lt;&lt;"-"&lt;&lt;cunix.GetMonth()&lt;&lt;"-"&lt;&lt;cunix.GetnDay()&lt;&lt;std::endl;

    return 0;

}
</pre>
&nbsp;