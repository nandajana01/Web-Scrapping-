.. code:: ipython3

    from bs4 import BeautifulSoup
    import requests
    import pandas as pd

.. code:: ipython3

    jumia = requests.get('https://www.jumia.co.ke/all-products/')

.. code:: ipython3

    jumia.content

.. code:: ipython3

    name_info = []
    price_info = []
    Rating_info = []

.. code:: ipython3

    for page in range(1,51):
        url = "https://www.jumia.co.ke/all-products/" + "?page" + str(page)+"#catalog-listing"
        furl = requests.get(url)
        jana = BeautifulSoup(furl.content, 'html.parser')
        products = jana.find_all('div', class_='info')
        
        for product in products:
            
            Name = product.find('h3', class_='name').text.replace('\n','')
            Price = product.find('div', class_='prc').text.replace('\n','')
            
            try:
                Rating = product.find('div', class_='star_s').text.replace('\n','')
            except:
                Rating = 'None'
                
            name_info.append(Name)
            price_info.append(Price)
            Rating_info.append(Rating)
            
            print(name_info, price_info, Rating_info)

.. code:: ipython3

    dict = {'product Name':name_info, 'price':price_info, 'Rating':Rating_info}

.. code:: ipython3

    sachin = pd.DataFrame(dict)

.. code:: ipython3

    sachin




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>product Name</th>
          <th>price</th>
          <th>Rating</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>Passport Passport Blended Scotch Whisky - 700Ml</td>
          <td>KSh 999</td>
          <td>None</td>
        </tr>
        <tr>
          <th>1</th>
          <td>NIVEA UV Face Shine Control Cream SPF 50 - 50ml</td>
          <td>KSh 799</td>
          <td>None</td>
        </tr>
        <tr>
          <th>2</th>
          <td>Garnier Even &amp; Matte Vitamin C Anti-Dark Spot ...</td>
          <td>KSh 1,280</td>
          <td>None</td>
        </tr>
        <tr>
          <th>3</th>
          <td>Garnier Pure Active 3 In1 Charcoal Anti Blackh...</td>
          <td>KSh 1,250</td>
          <td>None</td>
        </tr>
        <tr>
          <th>4</th>
          <td>AILYONS FK-0301 Stainless Steel 1.8L Electric ...</td>
          <td>KSh 809</td>
          <td>None</td>
        </tr>
        <tr>
          <th>...</th>
          <td>...</td>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <th>1995</th>
          <td>Deliya Hair Blow Dryer 2200W Hair Dryer+6 Gifts</td>
          <td>KSh 1,756</td>
          <td>None</td>
        </tr>
        <tr>
          <th>1996</th>
          <td>XIAOMI Redmi 9A, 6.53", 2GB+32GB, 13.0MP, 5000...</td>
          <td>KSh 10,299</td>
          <td>None</td>
        </tr>
        <tr>
          <th>1997</th>
          <td>Garnier Anti-Blemish Charcoal Serum With AHA +...</td>
          <td>KSh 1,650</td>
          <td>None</td>
        </tr>
        <tr>
          <th>1998</th>
          <td>ARHANORY Ladies Purse Wallets Women Leather Wa...</td>
          <td>KSh 449</td>
          <td>None</td>
        </tr>
        <tr>
          <th>1999</th>
          <td>Waanzilish Mens Sneakers Shoes Outdoor Hiking ...</td>
          <td>KSh 1,200</td>
          <td>None</td>
        </tr>
      </tbody>
    </table>
    <p>2000 rows × 3 columns</p>
    </div>



.. code:: ipython3

    sachin.to_csv('product_from_jumia.csv', index=False, encoding = 'utf-8')




