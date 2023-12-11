<% '生成随机7位数字
randomize
dim a1,a2,a3,a4,a5,a6,a7
a1=Int(10 * Rnd)

a2=Int(10 * Rnd)
do while a1=a2
a2=Int(10 * Rnd)
loop

a3=Int(10 * Rnd)
do while a1=a3 or a2=a3
a3=Int(10 * Rnd)
loop

a4=Int(10 * Rnd)
do while a1=a4 or a2=a4 or a3=a4
a4=Int(10 * Rnd)
loop

a5=Int(10 * Rnd)
do while a1=a5 or a2=a5 or a3=a5 or a4=a5
a5=Int(10 * Rnd)
loop

a6=Int(10 * Rnd)
do while a1=a6 or a2=a6 or a3=a6 or a4=a6  or a5=a6
a6=Int(10 * Rnd)
loop

a7=a1&a2&a3&a4&a5&a6
%>