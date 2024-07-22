- column별로 데이터 뽑기
	```js
	const columns = grid[0].map((column, index) => (
		grid.map((row) => (
			row[index]
		))
	))
	
	```