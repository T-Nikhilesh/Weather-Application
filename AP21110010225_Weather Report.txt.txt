from bs4 import BeautifulSoup
import requests
import json

headers = {
    'User-Agent':
           'Mozilla/5.0 (Windows NT 10.0;Win64; x64) AppleWebKit/537.36 (KHTML, likeGecko) Chrome/58.0.3029.110 Safari/537.3'}

def weather(city):
    city = city.replace(" ", "+")
    res = requests.get(f'https://www.google.com/search?q={city}&oq={city}&aqs=chrome.0.35i39l2j0l4j46j69i60.6128j1j7&sourceid=chrome&ie=UTF-8',headers=headers)
    
    print("Searching...\n")
    
    soup = BeautifulSoup(res.text, 'html.parser')
    
    location = soup.select('#wob_loc')[0].getText().strip()
    time = soup.select('#wob_dts')[0].getText().strip()
    info = soup.select('#wob_dc')[0].getText().strip()
    weather = soup.select('#wob_tm')[0].getText().strip()
    prec = soup.select('#wob_pp')[0].getText().strip()
    hum = soup.select('#wob_hm')[0].getText().strip()
    wind = soup.select('#wob_ws')[0].getText().strip()
    
    print(location)
    print("Last Updated ",time)
    print(info)
    print("Temperature =", weather+"°C")
    print("precipitation =",prec)
    print("humidity =" , hum)
    print("wind =", wind)
    
    d={}
    
    d['location']=location
    d['time']=time
    d['info']=info
    d['weather']=weather
    d['precipitation']=prec
    d['humidity']=hum
    d['wind']=wind
    
    data=[]

    with open('sample.json', "r") as f:
        data=json.load(f)
    
    data.append(d)

    with open('sample.json', "w") as f:
        json.dump(data, f, indent = 4, ensure_ascii=True)

def his():
    with open('sample.json', "r") as f:
        info = json.load(f)
        print(json.dumps(info, indent = 4))

username=input("Enter your name: ")
city = input("Enter your city: ")
city = city+" weather"

weather(city)

c=input("do you want to see history of the search resluts Y/n = ")
c.lower()
if(c=="y"):
    his()
else:
    pass

print("Have a Nice Day",username,":)")