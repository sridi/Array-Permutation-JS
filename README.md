# Array-Permutation-JS
Algorithms to get the permutation of a given array

### Iterative Solution
```javascript
var permutation = (a,r=[]) => {
	if(!a.length) return a
	else if(!r.length){
        	for(i=0;i<a.length;i++){
        		r.push({v:[a[i]],n:a.removeIndex(i)})
        	}
	}else{
		for(i=0;i<r.length;i++){
			for(j=0;j<r[0].n.length;j++){
				r.push({v:r[0].v.append(r[0].n[j]),n:r[0].n.removeIndex(j)})
			}
			if(r[0].n.length>0) r.shift()
		}
	}
	if(r[0].n.length) return permutation(a,r)
	return r.map(e=>e.v)
}
```

### Backtracking Solution
```javascript
var permutation = (a,r=[],n=0,c=-1) => {
	if(!a.length||(a.length&&n==a.length&&!r[c].e.length&&r[c].p==null)){
		return !a.length?[]:r.filter(x=>x.v.length==a.length).map(x=>x.v)
	}else if(!r.length||(!r[c].e.length&&r[c].p==null)){
		r.push({v:[a[n]],e:a.removeIndex(n),p:null})
		return permutation(a,r,++n,r.length-1) // move root
	}else if(!r[c].e.length){
		return permutation(a,r,n,r[c].p) // backtracking
	}else{
		x=r[c].e.shift()
		v=r[c].v.append(x)
		r.push({v:v,e:a.substract(v),p:c})
		return permutation(a,r,n,r.length-1) // explore
	}
}
```
### Helpers
```javascript
Array.prototype.removeIndex = function(i){
	return this.slice(0,i).concat(this.slice(i+1,this.length))
}

Array.prototype.append = function(e){
	return this.concat([e])
}

Array.prototype.substract = function(a){
	return this.filter(x=>!a.includes(x))
}
```
