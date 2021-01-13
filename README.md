# Scraping.py

## 컴퓨터공학 포토폴리오 김지우
### 온라인compiler site인 Repl.it 에서 기존에 만들어 논 것 입니다.
### https://repl.it/@Jiwoowoo1/Scrapingpy#main.py <repl.it 주소>

## Job scrapper

구직 싸이트로 유명한 indeed에서 BeautifulSoup로 추출하여 뽑아오는 형식으로 구성하였습니다.
   
![2021-01-12 11-57-58 mkv_000002750](https://user-images.githubusercontent.com/60593969/104438158-05480b00-55d3-11eb-8465-e09c651e8349.gif)

## 최대 페이지수 확인

```python
def get_last_page(url):  // 인자로 들어온 indeed url
    result = requests.get(url) // requests요청
    soup = BeautifulSoup(result.text, "html.parser") // parser 구문 분석기를 통해 구문 분석 
    pagination = soup.find("div", {"class": "pagination"}) // soup에서 html element를 통해 class부분 추출
    links = pagination.find_all('a') // 찾은 pagination에서 링크를 모두 찾음
    pages = []
    for link in links[:-1]: // 반복적으로 링크에서 span 요소를(페이지수) 찾아내어 string으로 변환후 저장
        pages.append(int(link.find("span").string))
    max_page = pages[-1] // 마지막값은 next버튼이라서 제거 
    return max_page // 최대 페이지수 반환
```
이를 통해 최대 몇페이지까지 있는지 먼저 확인한다.

