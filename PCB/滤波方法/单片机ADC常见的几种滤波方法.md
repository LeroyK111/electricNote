## 一、限幅滤波

**1、方法**

- 根据经验判断两次采样允许的最大偏差值A
    
- 每次采新值时判断：若本次值与上次值之差<=A，则本次有效；若本次值与上次值之差>A，本次无效，用上次值代替本次。
    

**2、优缺点**

- 克服脉冲干扰，无法抑制周期性干扰，平滑度差。
    

**3、代码**

```
/* A值根据实际调，Value有效值，new_Value当前采样值，程序返回有效的实际值 */#define A 10char Value;char filter(){  char new_Value;  new_Value = get_ad();                                        //获取采样值  if( abs(new_Value - Value) > A)   return Value;             //abs()取绝对值函数  return new_Value;}
```

## 二、中位值滤波

**1、方法**

- 连续采样N次，按大小排列
    
- 取中间值为本次有效值
    

**2、优缺点**

- 克服波动干扰，对温度等变化缓慢的被测参数有良好的滤波效果，对速度等快速变化的参数不宜。
    

**3、代码**

```
#define N 11char filter(){ char value_buf[N]; char count,i,j,temp; for(count = 0;count < N;count++)                                //获取采样值 {  value_buf[count] = get_ad();  delay(); } for(j = 0;j<(N-1);j++)  for(i = 0;i<(n-j);i++)  if(value_buf[i]>value_buf[i+1])  {   temp = value_buf[i];   value_buf[i] = value_buf[i+1];   value_buf[i+1] = temp;  } return value_buf[(N-1)/2];}
```

## 三、算数平均滤波

**1、方法**

- 连续采样N次，取平均
    
- N较大时平滑度高，灵敏度低
    
- N较小时平滑度低，灵敏度高
    
- 一般N=12
    

**2、优缺点**

- 适用于存在随机干扰的系统，占用RAM多，速度慢。
    

**3、代码**

```
#define N 12char filter(){ int sum = 0; for(count = 0;count<N;count++)  sum += get_ad(); return (char)(sum/N);}
```

## 四、递推平均滤波

**1、方法**

- 取N个采样值形成队列，先进先出
    
- 取均值
    
- 一般N=4~12
    

**2、优缺点**

- 对周期性干扰抑制性好，平滑度高
    
- 适用于高频振动系统
    
- 灵敏度低，RAM占用较大，脉冲干扰严重
    

**3、代码**

```
/* A值根据实际调，Value有效值，new_Value当前采样值，程序返回有效的实际值 */#define A 10char Value;char filter(){  char new_Value;  new_Value = get_ad();                                        //获取采样值  if( abs(new_Value - Value) > A)   return Value;             //abs()取绝对值函数  return new_Value;}
```

## 五、中位值平均滤波

**1、方法**

- 采样N个值，去掉最大最小
    
- 计算N-2的平均值
    
- N= 3~14
    

**2、优缺点**

1. 融合了中位值，平均值的优点
    
2. 消除脉冲干扰
    
3. 计算速度慢，RAM占用大
    

**3、代码**

```
char filter(){ char count,i,j; char Value_buf[N]; int sum=0; for(count=0;count<N;count++)  Value_buf[count]= get_ad(); for(j=0;j<(N-1);j++)  for(i=0;i<(N-j);i++)   if(Value_buf[i]>Value_buf[i+1])   {     temp = Value_buf[i];     Value_buf[i]= Value_buf[i+1];      Value_buf[i+1]=temp;   }   for(count =1;count<N-1;count++)    sum += Value_buf[count];   return (char)(sum/(N-2));}
```

## 六、限幅平均滤波

**1、方法**

- 每次采样数据先限幅后送入队列
    
- 取平均值
    

**2、优缺点**

- 融合限幅、均值、队列的优点
    
- 消除脉冲干扰，占RAM较多
    

**3、代码**

```
#define A 10#define N 12char value,i=0;char value_buf[N];char filter(){ char new_value,sum=0; new_value=get_ad(); if(Abs(new_value-value)<A)  value_buf[i++]=new_value; if(i==N)i=0; for(count =0 ;count<N;count++)  sum+=value_buf[count]; return (char)(sum/N);}
```

## 七、一阶滞后滤波

**1、方法**

- 取a=0~1
    
- 本次滤波结果=（1-a）* 本次采样 + a * 上次结果
    

**2、优缺点**

- 良好一直周期性干扰，适用波动频率较高场合
    
- 灵敏度低，相位滞后
    

**3、代码**

```
/*为加快程序处理速度，取a=0~100*/#define a 30char value;char filter(){ char new_value; new_value=get_ad(); return ((100-a)*value + a*new_value);}
```

## 八、加权递推平均滤波

**1、方法**

- 对递推平均滤波的改进，不同时刻的数据加以不同权重，通常越新的数据权重越大，这样灵敏度高，但平滑度低。
    

**2、优缺点**

- 适用有较大滞后时间常数和采样周期短的系统，对滞后时间常数小，采样周期长、变化慢的信号不能迅速反应其所受干扰。
    

**3、代码**

```
/* coe数组为加权系数表 */#define N 12char code coe[N]={1,2,3,4,5,6,7,8,9,10,11,12};char code sum_coe={1+2+3+4+5+6+7+8+9+10+11+12};char filter(){ char count; char value_buf[N]; int sum=0; for(count=0;count<N;count++) {  value_buf[count]=get_ad(); } for(count=0;count<N;count++)  sum+=value_buf[count]*coe[count]; return (char)(sum/sum_coe);}
```

## 九、消抖滤波

**1、方法**

- 设置一个滤波计数器
    
- 将采样值与当前有效值比较
    
- 若采样值=当前有效值，则计数器清0
    
- 若采样值不等于当前有效值，则计数器+1
    
- 若计数器溢出，则采样值替换当前有效值，计数器清0
    

**2、优缺点**

- 对变化慢的信号滤波效果好，变化快的不好
    
- 避免临界值附近的跳动，计数器溢出时若采到干扰值则无法滤波
    

**3、代码**

```
#define N 12char filter(){ char count=0,new_value; new_value=get_ad(); while(value!=new_value) {  count++;  if(count>=N) return new_value;  new_value=get_ad(); } return value;}
```

## 十、限幅消抖滤波

**1、方法**

- 先限幅 后消抖
    

**2、优缺点**

- 融合了限幅、消抖的优点
    
- 避免引入干扰值，对快速变化的信号不宜
    

**3、代码**

```
#define A 10#define N 12char value;char filter(){ char new_value,count=0; new_value=get_ad(); while(value!=new_value) {  if(Abs(value-new_value)<A)  {  count++;  if(count>=N) return new_value;  new_value=get_ad();  } return value; }}
```