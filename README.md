import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

class Product {
  String name;
  double price;

  Product({this.name, this.price});

  factory Product.fromJson(Map<String, dynamic> json) {
    return Product(name: json['name'], price: json['price']);
  }
}

class ProductListPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final List<Product> products = await getProducts();
    return ListView.builder(
      itemCount: products.length,
      itemBuilder: (context, index) {
        return Card(
          child: ListTile(
            title: Text(products[index].name),
            subtitle: Text(products[index].price.toString()),
          ),
        );
      },
    );
  }
}

Future<List<Product>> getProducts() async {
  final String url = "https://example.com/api/products";
  final Response response = await http.get(url);
  if (response.statusCode == 200) {
    final List<Product> products = (json.decode(response.body) as List)
        .map((product) => Product.fromJson(product))
        .toList();
    return products;
  } else {
    return null;
  }
}
