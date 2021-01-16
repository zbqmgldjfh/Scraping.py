# Scraping.py

## 컴퓨터공학 포토폴리오 김지우
### 온라인compiler site인 Repl.it 에서 기존에 만들어 논 것 입니다.
### https://repl.it/@Jiwoowoo1/Scrapingpy#main.py <repl.it 주소>

## Job scrapper

구직 싸이트로 유명한 indeed에서 BeautifulSoup로 추출하여 뽑아오는 형식으로 구성하였습니다.
   
![2021-01-12 11-57-58 mkv_000002750](https://user-images.githubusercontent.com/60593969/104438158-05480b00-55d3-11eb-8465-e09c651e8349.gif)

## Code 리뷰

### get_last_page (최대 페이지수 확인)

```python
def get_last_page(url):  # 인자로 들어온 indeed url
    result = requests.get(url) 
    soup = BeautifulSoup(result.text, "html.parser") # parser 구문 분석기를 통해 구문 분석 
    pagination = soup.find("div", {"class": "pagination"}) # div를 찾아 class명이 pagination인 요소를 반환 
    links = pagination.find_all('a') # 찾은 pagination에서 링크를 모두 찾음
    pages = []
    for link in links[:-1]: # 반복적으로 링크에서 span 요소를(페이지수) 찾아내어 string으로 변환후 저장
        pages.append(int(link.find("span").string))
    max_page = pages[-1] # 마지막값은 next버튼이라서 -1 
    return max_page // 최대 페이지수 반환
```
이를 통해 최대 몇페이지까지 있는지 먼저 확인한다.

___

###  extract_job (job 추출함수)

```python
def extract_job(html): # html을 인자로 받는다.
    title = html.find("h2", {"class": "title"}).find("a")["title"] # h2를 찾아 title class안의 anchor의 title 하나만 찾음
    company = html.find("span", {"class": "company"}) # span element의 company class에서 회사목록 추출
    if company: # company링크가 있다면
        company_anchor = company.find("a")
        if company_anchor is not None:
            company = str(company_anchor.string)
        else:
            company = str(company.string)
        company = company.strip()
    else: // company링크가 없다면
        company = None
    location = html.find("div", {"class": "recJobLoc"})["data-rc-loc"]
    job_id = html["data-jk"]
    return{
        'title': title,
        'company': company,
        'location': location,
        "link": f"https://kr.indeed.com/viewjob?jk={job_id}"
    }
```

위의 코드에서 if문으로 company의 링크가 있는지에 따라 나눴다. 놀랍게도 링크가 없는 회사가 존제하였다.
원래는 다음과 같이 링크가 있어야 정상이다. 
<img src = "https://user-images.githubusercontent.com/60593969/104808969-0ffddc80-582d-11eb-92f4-4e360af10479.png" width="500px">

하지만 없는 회사가 있었다.
<img src = "https://user-images.githubusercontent.com/60593969/104808972-112f0980-582d-11eb-91c7-b73eefe4b62c.png" width="500px">

