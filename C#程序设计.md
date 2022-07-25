# 模型层

## 武器

- #### 武器名字

- #### 武器的属性加成

	- ```C#
		SkillProperties SkillPropertiesBuff(){
			// 力 智 精 体
			int power;
			int intterl;
			int mind;
			int strength;
			
			power = 随机();
			intterl = random();
			mind = randowm();
			strength = random();
			
			return new SkillProperty(power, innterl, mind, strength);
		}
		```

		

- #### 攻击力 / 防御力

	- 最低数值  ~  最高数值随机

		- 正态 随机算法

			- μ =  最高线即 max - min / 2

			- σ =  x  -  μ

			- ```C#
				int getRandowmValues(int min, int max){
					double u = (min+max) / 2;
					double v = 5;
					double x = random(min, max );
					double a = - (x - u) * (x - u) / (2 * v * v) ;
					double b = 1 / ( sqrt(2*pi) * v);
					double c = b * exp(a);
					
					int res = (int) c;
				}
				```

				

			- 

- #### 武器类型

	- 刀
		- 主力体
	- 剑
		- 主力精
	- 弓
		- 主力智
	- 杖
		- 主智精

## 角色

- ##### 力智精体

- ##### 功防速暴

- ##### 行动条

	- ###### 一回合+1， 满值 2 ， 受一次攻击 + 1

	- ```c#
		int t;
		t ~ (0 , 2 );
		when 2
		you can skill
		```

		

## 其他扩展

- ##### 速度条

	- 队列，先进先出

- ##### ~~怒气条~~

	- 必杀技释放前提条件

## 敌人

- ##### 攻防速暴

- ##### 蓄力速度

- ##### 蓄力条（控制层实现）



# 业务层（连接数据库）

## 1、 接收数据

## 2、 发送数据

## 3、 搜索数据



# 控制层

## 1、 页面控制

## 2、 模型控制

> ###  战斗控制

​	战斗顺序：开始=>读取速度=>计算行动对象

### b. 数据计算





# 额外内容：

#### 1、序列化

#### 2、绑定

