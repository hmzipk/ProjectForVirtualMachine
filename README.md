# ProjectForVirtualMachine
from flask import Flask, jsonify
from flask import make_response
from flask import request

#Kitap Öneri API si

products = [
    {
        'id': 1,
        'kitap adi': 'Kablolardaki Hayalet',
        'yazar': 'Kevin Mitnick',
        'tur': 'Biyografi'
    },
    {
        'id': 2,
        'kitap adi': 'Hayvan Ciftligi',
        'yazar': 'George Orwell',
        'tur': 'Roman'
    },
    {
        'id': 3,
        'kitap adi': 'Insan Neyle Yasar?',
        'yazar': 'Nev Nikolayevic Tolstoy',
        'tur': 'Rus Edebiyati'
    },
    {
        'id': 4,
        'kitap adi': 'Donusum',
        'yazar': 'Franz Kafka',
        'tur': 'Alman Edebiyati'
    }
]

app = Flask(__name__)


@app.route('/hamza/api/kitaplar', methods=['GET'])
def get_products():
    return jsonify({'products': products})


@app.route('/hamza/api/kitaplar/<int:product_id>', methods=['GET'])
def get_product(product_id):
    product = [product for product in products if product['id'] == product_id]
    if len(product) == 0:
        return jsonify({'product': 'Not found'}), 404
    return jsonify({'product': product})


@app.route('/hamza/api/kitaplar', methods=['POST'])
def create_product():
    newProduct = {
        'id': products[-1]['id'] + 1,
        'kitap adi': request.json['kitap adi'],
        'yazar': request.json['description'],
        'tur': request.json['tur']
    }
    products.append(newProduct)
    return jsonify({'product': newProduct}), 201


@app.route('/hamza/api/kitaplar/<int:product_id>', methods=['DELETE'])
def delete_product(product_id):
    product = [product for product in products if product['id'] == product_id]
    if len(product) == 0:
        return jsonify({'product': 'Not found'}), 404
    products.remove(product[0])
    return jsonify({'result': True})


@app.errorhandler(404)
def not_found(error):
    return make_response(
        jsonify({'HTTP 404 Error': 'The content you looks for does not exist. Please check your request.'}), 404)


if __name__ == '__main__':
    app.run(debug=True)
