# study-record
good good study day day up

2.28  
forcing term in LBM  
（一）力的分类  
1.外力  
包括重力、浮力等  
 

2.分子间作用力（多相多组分流）  
LBM通过增加流体与固体的作用来区分相  
常用有Shan-Chen模型（自下而上，用相对作用势直接模拟不同相和组分之间的相互作用，用粒子相互作用来描述相与相之间的表面效应；）、free energy（自上而下，通过热力学函数反应分子间作用力）  
Shan-Chen模型  
非局部作用通过势(potential)来描述：格林函数，有效密度  
LBM只考虑相邻最近粒子的相互作用，模量系数表示相互作用力的强度，正负号表示方向，正为吸引，负为排斥  
计算步骤：压强分布——其余未知数——F 


（二）forcing term的表现形式  
1.加在分布函数上，A是常数，S是多项式（Guo（针对LGBK，也可以推广到MRT), He)  
2.加在平衡态分布函数上，A不是常数，S等于0（Shan-Chen)  


3.3
多松弛模型 MRT-LBM  
1. 速度空间  
f，streaming在速度空间中进行
2. 矩阵空间  
m=Mf，通过这个式子，从速度空间转换到矩阵空间  
M是转换矩阵，collision在矩阵空间中进行  
m是相当于f的分布函数，meq是矩阵空间中的平衡态矩，S是对角矩阵，S中有多个松弛时间，如果1/taoi=1/tao,MRT退化成LGBK  
collision完成之后通过m=Mf反过去得到f  
然后在速度空间中完成streaming的传递  


4.7 palabos 编程  
（1）结论1：基本上所有情况下，int和unsigned int都被替换为plint与pluint。  

原因：因为32位系统无法计算太大，超过2g的数据集不太够了，这样的替换都是为了方便在64位下计算的。而64位系统下，输入int还会定义32字节的类型。所以就用plint和pluint来表示64字节类型。  

(2) 结论2：输入缓冲cout都被替换为pcout，cerr被替换成pcerr，clog被替换成pclog，对于文件输出ofstream，被替换成plb_ofstream。  

原因：在MPI并行计算时，每一条线路都使用cout输出，为了保持程序运行的连贯性，才替换的。  

(3) 选择什么数据类型来存储数组值呢？答案是看数组值的大小。  
像LBM的粒子群这种大的数据集，则使用palabos的 BlockLattice 类型。这类程序差不多长这样：  
// Instantiate a nxnynz D3Q19 lattice with double-precision  
// floating point numbers.   
MultiBlockLattice3D<double,D3Q19descriptor> lattice(nx,ny,nz);  
// Instantiate a nxnynz matrix of double-precision floating  
// point scalars.  
MultiScalarField3D field(nx,ny,nz);  

像小的非并行计算的，比如说存储计算参数这样的不是巨大的数据集，C++标准库的数据类型 _vector_即可初始化动态尺寸的数组。差不多长这样：  
plint numspecies = 5;  
// Instantiate a vector of double-precision floating point  
// numbers with 5 elements.  
vector viscosities(numSpecies);  
viscosities[3] = 0.63;  

对于非常小的非并行计算的而且确定尺寸的数组，palabos定义了array，通常用于存储小的物理向量，比如说速度。大概长这样：  
// Instantiate a fixed-size array of type double and with  
// 3 elements. The values can be initialized directly in  
// the constructor for arrays of size 2 and 3.  
Array<double,3> velocity(0., 0., 0.5);  
velocity[0] = 0.1;  

(4）速度和密度
1）LB中V和ρ在宏观尺度上是最常见的变量，但在palabos代码里你会发现，这两个量很少见。在palabos里面，常用到的是 rhoBar 和 j 。

变量 j ：first-order velocity moment,一阶速度矩，大部分模型下，流体动量 j ＝ rho * u。
变量 rhoBar ：这个可以自定义，默认情况下它被定义为 rhoBar=rho-1 ，用于提升计算精度。因为液体密度经常接近1，这样密度减去1的话，数值就是在0上下，表达双精度点的变量的字节也更有意义。

4.20 
论文Modeling condensation on structured surfaces using lattice Boltzmann method
how to incorporate temperature dependence
1. double distribution function  

2. continum approach，直接解宏观thermal energy equation，在空间和时间离散化之后得到internal energy 的显式更新，并且把它当作主要参数，也就是说不用再理念国外引进独立的源项    
+ fluid satisfy van der waals equation of state
+ Peng-Robinson equation of state
为什么不用Peng-Robinson equation of state？
First, for solving the energy equation we use a conserved quantity formulation, with rho*e as the primary variable. Since the specific internal energy function for a van der Waals fluid is linear, this allows for direct computation of the heat flux from e and rho.即热流和主要参数之间存在线性关系比较好计算  

Second, the condensation results presented in Section 4.3 have several assumptions that make the results qualitative when compared to water condensation. Specifically, 2D DWC condensation rates of a pure vapor at low density ratios will not exactly carry over to experiments that are inherently 3D at high density ratios. Consequently, van der Waals equation of state is sufficient for qualitatively capturing the essential behavior of a non-ideal fluid.纯蒸汽在低密度比下的二维DWC凝结率不会完全延续到高密度比下固有的三维实验中。因此，范德瓦尔斯状态方程足以定性地捕捉到非理想流体的基本行为。  

4.24  
speed of sound is c, which is different from the isothermal speed of sound cs  
speed of sound is determined by EOS, 压力对密度在处entropy的偏导再开根号，代表actual speed of sound for a given pressure    


