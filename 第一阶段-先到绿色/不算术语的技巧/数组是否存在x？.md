不要写 arr.counter(x)
时间复杂度为 o(N)
这步骤还经常写在循环到时候循环时间复杂度就为o(N* M)了

用set，先把数组转为set
然后 if i in set
时间复杂度为o(1)

