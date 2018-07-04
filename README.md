# Arithmetic Operations
Javascript algorithms for calculating arithmetic operations for big numbers given as strings

## Addition
```javascript
const add = (a, b) => {
	if(a.length>=b.length){
		a=a.split('').reverse(),b=b.split('').reverse(),r=[],c=0
		a.map((e,i)=>{
		    if(i<b.length){
			s=(+e + +b[i] + +c).toString()
			r[i]=s.slice(s.length-1,s.length)
			c=s.slice(0,s.length-1)
		    }else{
			s=(+e + +c).toString()
			r[i]=s.slice(s.length-1,s.length)
			c=s.slice(0,s.length-1)
		    }
		})
        return c+r.reverse().join('')
	}else{
		return add(b, a)
	}
}
```

## Substraction
```javascript
const substract = (a,b,c=2,s='') => {
	if(c>1) c=a.comparedTo(b)
	if(c==1) {
		a=a.split('').reverse(),b=b.split('').reverse(),r=[],n='0'
		a.map((e,i)=>{
			if(i<b.length){
				d=(+e >= (+b[i] + +n))?'0':'1'
				r[i]=(+(d + e) - (+b[i] + +n)).toString()
				n=d
			}else{
				d=(+e >= +n)?'0':'1'
				r[i]=(+(d + e) - +n).toString()
				n=d
			}
		})
		return s+r.reverse().join('').replace(/^0+/,'')
	} else if(c==-1) return substract(b,a,1,'-')
	else return '0'
}
```

## Multiplication
```javascript
const multiply = (a, b) => {
	if(a=="0"||b=="0") return "0"
	else{
		a=a.split('').reverse(),
		b=b.split('').reverse(),
		n=new Array(a.length),
		r=new Array(a.length+b.length).fill(0)
		a.map((x,i)=>{
			n[i]=new Array(a.length+b.length).fill(0)
			b.map((y,j)=>{
				m = +x * +y
				if(m < 10) n[i][i+j] = m.toString()
				else{
					m = m.toString()
					n[i][i+j] = m.slice(m.length-1,m.length)
					m = m.slice(0,m.length-1)
					r[i+j+1] = (+r[i+j+1] + +m).toString()
				}
			})
		})
		for(i=0;i<a.length;i++){
			for(j=0;j<b.length;j++){
				r[i+j] = (+r[i+j] + +n[i][i+j]).toString()
			}
		}
		for(c=0,i=0;i<r.length-1;i++){
			m = r[i]
			if(+m>9){
				r[i] = m.slice(m.length-1,m.length)
				m = m.slice(0,m.length-1)
				r[i+1] = (+r[i+1] + +m).toString()
			}
		}
		return r.reverse().join('')
	}
}
```

## Division
```javascript
const divide = (a,b,p=2) => {
	r=[0,0],d=0
	if(b=="0") return "Division by 0 impossible"
	else if(a=="0") return "0"
	else{
		while(a>=b){
			if(!d){
				a-=b
				r[0]++
				if(a<b&&p){
					d=1
					a*=10
				}
			}else{
				a-=b
				r[1]++
				if(a<b&&p>1){
					p--
					a*=10
					r[1]*=10
				}
			}
		}
		return +r[1]==0?r[0].toString():r[0]+"."+r[1]
	}
}
```

# Combinatorial
Javascript algorithms for generating mathematical combinations from a given array

## Permutation
### Iterative Solution
```javascript
const permutation = (a, p, r=[]) => {
	if(!a.length) return "Permutation impossible"
	else if(a.length == 1) return [a]
	else if(p<2&&r.length) return r
	else{
		n=[]
		if(!r.length){
			for(i=0;i<a.length;i++){
				n=a.map(x=>[x])
			}
			p=a.length+1
		}else{
			for(i=0;i<r.length;i++){
				for(j=0;j<a.length;j++){
					!r[i].includes(a[j])&&n.push(r[i].append(a[j]))
				}
			}
		}
		return permutation(a, --p, n)
	}
}
```

### Backtracking Solution
```javascript
const permutation = (a,r=[],n=0,c=-1) => {
	if(!a.length||(a.length&&n==a.length&&!r[c].e.length&&r[c].p==null)){
		return !a.length?"Permutation impossible":r.filter(x=>x.v.length==a.length).map(x=>x.v)
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

## Arrangements
### Arrangement without repetition
```javascript
const arrangement = (a, p, r=[]) => {
	if(!a.length || p>a.length || (!r.length&&p<1)) return "Impossible Arrangement"
	else if(p<2&&!r.length) return a.map(x=>[x])
	else if(p<2&&r.length) return r.map(x=>x.v)
	else{
		n=[]
		if(!r.length){
			for(i=0;i<a.length;i++){
				n.push({v:[a[i]],e:a.removeIndex(i)})
			}
			p++
		}else{
			for(i=0;i<r.length;i++){
				for(j=0;j<r[i].e.length;j++){
					n.push({v:r[i].v.append(r[i].e[j]),e:r[i].e.removeIndex(j)})
				}
			}
		}
		return arrangement(a, --p, n)
	}
}
```

### Arrangement with repetition
```javascript
const arrangement = (a, p, r=[]) => {
	if(!a.length || (!r.length&&p<1)) return "Impossible Arrangement"
	else if(p<2&&!r.length) return a.map(x=>[x])
	else if(p<2&&r.length) return r
	else{
		n=[]
		if(!r.length){
			for(i=0;i<a.length;i++){
				n=a.map(x=>[x])
			}
			p++
		}else{
			for(i=0;i<r.length;i++){
				for(j=0;j<a.length;j++){
					n.push(r[i].append(a[j]))
				}
			}
		}
		return arrangement(a, --p, n)
	}
}
```

## Combinations
### Combination without repetition
```javascript
const combination = (a, p, r=[]) => {
	if(!a.length || p>a.length || (p<1&&!r.length)) return "Impossible Combination"
	else if(p<2&&!r.length) return a.map(x=>[x])
	else if(r.length&&r[0].v.length==p) return r.map(x=>x.v)
	else if(p==a.length) return [a]
	else{
		n=[]
		if(!r.length){
			n=a.map((x,i)=>{return {v:[x],o:i}})
		}else{
			for(i=0;i<r.length;i++){
				for(j=r[i].o+1;j<a.length;j++){
					n.push({v:r[i].v.append(a[j]),o:j})
				}
			}
		}
		return combination(a, p, n)
	}
}

```

### Combination with repetition
```javascript
const combination = (a, p, r=[]) => {
	if(!a.length || (p<1&&!r.length)) return "Impossible Combination"
	else if(p<2&&!r.length) return a.map(x=>[x])
	else if(r.length&&r[0].v.length==p) return r.map(x=>x.v)
	else if(p==a.length) return [a]
	else{
		n=[]
		if(!r.length){
			n=a.map((x,i)=>{return {v:[x],o:i}})
		}else{
			for(i=0;i<r.length;i++){
				for(j=r[i].o;j<a.length;j++){
					n.push({v:r[i].v.append(a[j]),o:j})
				}
			}
		}
		return combination(a, p, n)
	}
}
```

### Helpers
```javascript
String.prototype.comparedTo = function(n){
	if(this.length>n.length) return 1
	else if(this.length<n.length) return -1
	else{
		for(let i=0;i<this.length;i++){
			if(+this[i] > +n[i]) return 1
			else if(+this[i] < +n[i]) return -1
		}
		return 0
	}
}

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
