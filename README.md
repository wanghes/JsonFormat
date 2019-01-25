# JsonFormat

### 格式化json,最终显示结果
<img src="http://img.mousecloud.cn/public/upload/1548382932959.jpeg" width="800" align="center" />

### 相关代码：
```html
<!DOCTYPE html>
<html>

<head>
	<title>测试一下</title>
	<style>
		pre {outline: 1px solid #ccc; padding: 5px; margin: 5px; }
 		.string { color: green; }
 		.number { color: darkorange; }
 		.boolean { color: blue; }
 		.null { color: magenta; }
		.key { color: red; }
	</style>
</head>

<body>
	<pre id="test"></pre>
	<script type="text/javascript">
		var testJson = {
			count: 1,
			start: 0,
			total: 250,
			subjects: [{
				rating: {
					max: 10,
					average: 9.6,
					stars: "50",
					min: 0
				},
				genres: [
					"犯罪",
					"剧情"
				],
				title: "肖申克的救赎",
				casts: [{
						alt: "https://movie.douban.com/celebrity/1054521/",
						avatars: {
							small: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p17525.webp",
							large: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p17525.webp",
							medium: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p17525.webp"
						},
						name: "蒂姆·罗宾斯",
						id: "1054521"
					},
					{
						alt: "https://movie.douban.com/celebrity/1054534/",
						avatars: {
							small: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p34642.webp",
							large: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p34642.webp",
							medium: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p34642.webp"
						},
						name: "摩根·弗里曼",
						id: "1054534"
					},
					{
						alt: "https://movie.douban.com/celebrity/1041179/",
						avatars: {
							small: "http://img3.doubanio.com/view/celebrity/s_ratio_celebrity/public/p5837.webp",
							large: "http://img3.doubanio.com/view/celebrity/s_ratio_celebrity/public/p5837.webp",
							medium: "http://img3.doubanio.com/view/celebrity/s_ratio_celebrity/public/p5837.webp"
						},
						name: "鲍勃·冈顿",
						id: "1041179"
					}
				],
				collect_count: 1624888,
				original_title: "The Shawshank Redemption",
				subtype: "movie",
				directors: [{
					alt: "https://movie.douban.com/celebrity/1047973/",
					avatars: {
						small: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p230.webp",
						large: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p230.webp",
						medium: "http://img1.doubanio.com/view/celebrity/s_ratio_celebrity/public/p230.webp"
					},
					name: "弗兰克·德拉邦特",
					id: "1047973"
				}],
				year: "1994",
				images: {
					small: "http://img1.doubanio.com/view/photo/s_ratio_poster/public/p480747492.webp",
					large: "http://img1.doubanio.com/view/photo/s_ratio_poster/public/p480747492.webp",
					medium: "http://img1.doubanio.com/view/photo/s_ratio_poster/public/p480747492.webp"
				},
				alt: "https://movie.douban.com/subject/1292052/",
				id: "1292052"
			}],
			title: "豆瓣电影Top250"
		}

		function repeat(s, count) {
			return new Array(count + 1).join(s);
		}

		function returnJsonHtml(json) {
			json = json.replace(/&/g, '&').replace(/</g, '<').replace(/>/g, '>');
			return json.replace(/("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\-]?\d+)?)/g, function(match) {
				var cls = 'number';
				if (/^"/.test(match)) {
					if (/:$/.test(match)) {
						cls = 'key';
					} else {
						cls = 'string';
					}
				} else if (/true|false/.test(match)) {
					cls = 'boolean';
				} else if (/null/.test(match)) {
					cls = 'null';
				}
				return '<span class="' + cls + '">' + match + '</span>';
			});
			return json;
		}

		function formatJson(str) {
			var json = str;
			var i = 0,
				len = 0,
				tab = "    ",
				targetJson = "",
				indentLevel = 0,
				inString = false,
				currentChar = null;


			for (i = 0, len = json.length; i < len; i += 1) {
				currentChar = json.charAt(i);

				switch (currentChar) {
					case '{':
					case '[':
						if (!inString) {
							targetJson += currentChar + "\n" + repeat(tab, indentLevel + 1);
							indentLevel += 1;
						} else {
							targetJson += currentChar;
						}
						break;
					case '}':
					case ']':
						if (!inString) {
							indentLevel -= 1;
							targetJson += "\n" + repeat(tab, indentLevel) + currentChar;
						} else {
							targetJson += currentChar;
						}
						break;
					case ',':
						if (!inString) {
							targetJson += ",\n" + repeat(tab, indentLevel);
						} else {
							targetJson += currentChar;
						}
						break;
					case ':':
						if (!inString) {
							targetJson += ": ";
						} else {
							targetJson += currentChar;
						}
						break;
					case ' ':
					case "\n":
					case "\t":
						if (inString) {
							targetJson += currentChar;
						}
						break;
					case '"':
						if (i > 0 && json.charAt(i - 1) !== '\\') {
							inString = !inString;
						}
						targetJson += currentChar;
						break;
					default:
						targetJson += currentChar;
						break;
				}
			}
			//document.form1.targetjson.value = ;
			return returnJsonHtml(targetJson);
		}


		window.onload = function() {
			var json = JSON.stringify(testJson)
			document.querySelector("#test").innerHTML = formatJson(json)
		}
	</script>

</body>

</html>
```
