# C++期末大作业报告
姓名：叶熙喆      
学号:18062041
#### 简述
此次作业的主要内容是对github上的开源代码nebula进行单元测试，并用git提交到github上。
#### git的安装
git的安装非常简单，只需要一行命令即可安装
```
sudo apt install git 
```
输入完成之后再输入自己的电脑密码即可开始安装

#### git 环境配置
用git创建远程连接需要在github里创建秘匙
##### 第一步
```
ssh-keygen -t rsa -C "993526915@qq.com"
```
使用ssh创建秘匙
##### 第二步
```
cd  ~/.ssh
```
进入.ssh目录下
##### 第三步
```
more id_rsa.pub
```
在id_rsa.pub文件中找到秘匙
![](2.png)
##### 第四步
将所有秘匙复制，并进入github，在左上角进入Settings-->SSH and GPG keys --> new ssh key 
将秘匙粘贴到Key中并保存
完成后会显示如下界面
最后，在终端输入
```
ssh -T git@github.com
```
#### nebula安装与启动
安装完git后就能用git从github上克隆代码到本地了
##### 第一步
进入nebula创建者的主页，fork到自己的主页下
```
git clone https://github.com/993526915/nebula.git
```
将代码拷贝到本地
##### 第二步
```
cd nebula
```
进入nebula文件夹
```
 ./build_dep.sh
```
安装依赖
```
source ~/.bashrc
```
```
mkdir build
```
创建build文件夹
```
cd build
```
进入build文件夹
```
cmake ..

make
```
等待一段时间直到完成

```
sudo make install
```
安装make下来的文件
```
cd /usr/local/nebula

bash> cp etc/nebula-graphd.conf.default etc/nebula-graphd.conf
bash> cp etc/nebula-metad.conf.default etc/nebula-metad.conf
bash> cp etc/nebula-storaged.conf.default etc/nebula-storaged.conf
```
这就配置完成了nebula
##### nebula使用
可以使用命令行来完成
创建tag edges和spaces等进行数据的储存与筛选

#### 源代码修改与编译
我在nebula/src/common/time/test文件夹下添加了名叫test.cpp的文件，并修改了cmakelist使得其在make后出现test_test文件

test.cpp
```c++

#include "base/Base.h"
#include <folly/Benchmark.h>
#include "time/Duration.h"
#include "time/WallClock.h"
#include "base/Base.h"
#include <gtest/gtest.h>
#include<vector>
#include<math.h>

 using nebula::time:: WallClock;

TEST(WallClock , time ) {

    std::vector<int64_t> timer;
    std::vector<int64_t>s;

    for (uint32_t i = 0; i < 10; i++) {

        auto sec=WallClock::fastNowInMicroSec();
        usleep(3000);
        auto sec2=WallClock::fastNowInMicroSec();
        auto cost_time=sec2-sec;
        timer.push_back(cost_time);
    }
    std::cout << timer[0] << std::endl;
    int64_t sum=std::accumulate(timer.begin(),timer.end(),0);
    double average=sum*1.0/10;
    std::cout << "Mean Error  : " << average-3000 << "ms "<<std::endl;

    for(int i = 0; i < 10 ; i++ ){
        int64_t a=(timer[i]-average)*(timer[i]-average);
        s.push_back(a);
    }

    int64_t sum2=std::accumulate(s.begin(),s.end(),0);
    double variance=sum2*1.0/9;
    std::cout << "The  Variance  Is : "<< variance << std::endl;
    double error=( abs(average-3000) * 1.0 / 3000) * 100;
    std :: cout << "Test Error Is  : " << error  << "%" << std::endl;

}

TEST(WallClock , time1 ) {

    std::vector<int64_t> timer;
    std::vector<int64_t>s;

    for (uint32_t i = 0; i < 10; i++) {

        auto sec=WallClock::slowNowInMicroSec();
        usleep(3000);
        auto sec2=WallClock::slowNowInMicroSec();
        auto cost_time=sec2-sec;
        timer.push_back(cost_time);
    }
    std::cout << timer[0] << std::endl;
    int64_t sum=std::accumulate(timer.begin(),timer.end(),0);
    double average=sum*1.0/10;
    std::cout << "Mean Error  : " << average-3000 << "ms "<<std::endl;

    for(int i = 0; i < 10 ; i++ ){
        int64_t a=(timer[i]-average)*(timer[i]-average);
        s.push_back(a);
    }

    int64_t sum2=std::accumulate(s.begin(),s.end(),0);
    double variance=sum2*1.0/9;
    std::cout << "The  Variance  Is : "<< variance << std::endl;
    double error=( abs(average-3000) * 1.0 / 3000) * 100;
    std :: cout << "Test Error Is  : " << error  << "%" << std::endl;

}

int main(int argc, char** argv) {

    testing::InitGoogleTest(&argc, argv);
    folly::init(&argc, &argv, true);
    google::SetStderrLogging(google::INFO);
    return RUN_ALL_TESTS();

}
```
这段代码是用来测试程序中代码的延迟，用到了WallClock类中的fastNowInMicroSec()函数计算usleep()函数的延迟误差，并给出了误差的均值以及延迟的方差。

![](6.png)

这是编译结果并且从结果可以看出实验结果确实如同预料，slowNowIn比MicroSec()的速度比fastNowInMicroSec()的速度慢，但精确程度要高
![](7.png)
编译结果得到验证，单元测试成功。  
### 实验心得

本次大作业，我了解了git的使用，同时更全面认识了github，懂得了如何用git提交代码，以及如何用github的commit与源代码作者进行交流，还懂得了如何cmake编译代码以及如何直接调用make完的代码。  
我感觉C++平常不好好学，是真的难顶。不过这个学期还是见识了一下，以后用得着的话，自己叶不会那么懵逼了。



