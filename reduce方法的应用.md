## reduce方法的运用

### 计算元素在数组中出现的次数

`let persons = ["蔡徐坤"，”陈立农“，”范丞丞“，”蔡徐坤“]
let nameObj = persons.reduce((pre,cur)=>{
if(cur in prev){
pre[cur]++
}else{
prev[cur]=1
}
return prev
,{})
console.log(nameObj)//{蔡徐坤：2，陈立农：1，范丞丞：1}`

### 数组去重
`let persons = ["蔡徐坤"，”陈立农“，”范丞丞“，”蔡徐坤“]
const resulArr = persons.reduce((pre,cur)=>{
if(!pre.includes(cur)){
return pre.concat(cur)
}else{ return pre }
},[])`

### 扁平化数组
`const myarr = [1,2,3,4,[1,2,3,4,[1,2,3,4]]]
const newArr = (arr) => {
return arr.reduce((prev,cur)=>{
return prev.concat(Array.isArray(cur)?newArr(cur):cur)
},[])
}
newArr(myarr);//[1,2,3,4,1,2,3,4,1,2,3,4]`
