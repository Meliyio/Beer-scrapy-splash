import scrapy
from scrapy_splash import SplashRequest

class BeerSpider(scrapy.Spider):
    name = 'beer'
    start_urls = ['https://www.beerwulf.com/en-gb/c/mixedbeercases']

    def start_requests(self):
        for url in self.start_urls:
            yield SplashRequest(url, self.parse, args={'wait': 2})

    def parse(self, response):
        for product in response.css('a.product.search-product.product-info-container.bw-plp-product-card.beer-torp.notranslate, a.product.search-product.product-info-container.bw-plp-product-card.draught-product.notranslate'):
            name = product.css('div.product-name::text').get()
            price = product.css('span.price::text').get()

            yield {
                'name': name.strip() if name else None,
                'price': price.strip() if price else None,
            }
