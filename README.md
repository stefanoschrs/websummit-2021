# WebSummit 2021

## Startups
> https://websummit.com/startups/startup-showcase

1. Load
	```javascript
	loadMore = async () => {
	    const loadMoreEl = document.querySelector('.ais-InfiniteHits-loadMore')
	    if (!loadMoreEl) return
	    
	    loadMoreEl.scrollIntoViewIfNeeded()
	    await sleep(2000)
	    loadMoreEl.click()
	    await sleep(1000)
	
	    return loadMore()
	}
	loadMore()
	```

2. Process
	```javascript
	[...document.querySelectorAll('.ais-InfiniteHits-item')].map((el) => {
	    let [location, tags] = el.querySelector('h4 + span')
		.innerText
		.split('|')
	    tags = (tags || '')
		.split(',')
		.reduce((acc, cur) => [...acc, ...cur.split('&')], [])
		.map((e) => e.trim().toLowerCase())
	    
	    const typeEl = el.querySelector('.shape-tl')
	    let type
	    if (typeEl) {
	        type = [...typeEl.classList]
			.pop()
			.toLowerCase()
	    }
	    
	    return {
	        name: el.querySelector('h4').innerText,
	        location,
	        tags,
	        type,
	        description: el.querySelector('h4 ~ div span').innerText,
	        logoUrl: el.querySelector('img').src
	    }
	})
	```

3. Results
	- [startups.json](./startups.json)
	- [startups.tsv](./startups.tsv)
	```javascript
	fs.writeFileSync('startups.tsv', JSON.parse(fs.readFileSync('startups.json', 'utf8')).map((e) => [e.name,e.type,e.location,e.tags.join(','),e.description,e.logoUrl].join('\t')).join('\n'))
	```
