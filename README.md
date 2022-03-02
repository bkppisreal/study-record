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
m是相当于f的分布函数，meq是矩阵空间中的平衡态矩，S是对角矩阵  
collision完成之后通过m=Mf反过去得到f  
然后在速度空间中完成streaming的传递  
