# flutterawesome.com

## 1.veggieseasons_adaptive

https://github.com/InMatrix/veggieseasons_adaptive?ref=flutterawesome.com



##### 1.DateFormat
```
var dateString = DateFormat('MMMM y').format(DateTime.now());

dateString = "March 2023"

```

##### 2.for
```
Widget _buildQuestionView(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(16),
      child: Column(
        children: [
          const SizedBox(height: 16),
          Text(
            currentTrivia.question,
            style: Theme.of(context).textTheme.bodyLarge,
          ),
          const SizedBox(height: 32),
          for (int i = 0; i < currentTrivia.answers.length; i++)
            Padding(
                padding: const EdgeInsets.all(8),
                child: FilledButton(
                  child: Text(
                    currentTrivia.answers[i],
                    textAlign: TextAlign.center,
                  ),
                  onPressed: () => null,
                )),
        ],
      ),
    );
  }
```

##### 3.for in
```
Row(
    mainAxisSize: MainAxisSize.max,
    children: [
      Text(
        widget.veggie.categoryName!.toUpperCase(),
        style: Theme.of(context).textTheme.labelLarge,
      ),
      const Spacer(),
      for (Season season in widget.veggie.seasons) ...[
        const SizedBox(width: 12),
        Padding(
          padding: Styles.seasonIconPadding[season]!,
          child: Icon(
            Styles.seasonIconData[season],
            semanticLabel: seasonNames[season],
            color: Styles.seasonColors[season],
          ),
        ),
      ],
    ],
  ),
```
##### 4.Table TableRow TableCell

```
Table(
    children: [
      TableRow(
        children: [
          TableCell(
            child: Text(
              'Serving size:',
              style: themeData.textTheme.bodyLarge,
            ),
          ),
          TableCell(
            child: Text(
              veggie.servingSize,
              textAlign: TextAlign.end,
              style: themeData.textTheme.bodyLarge,
            ),
          ),
        ],
      ),
      TableRow(
        children: [
          TableCell(
            child: Text(
              'Calories:',
              style: themeData.textTheme.bodyLarge,
            ),
          ),
          TableCell(
            child: Text(
              '${veggie.caloriesPerServing} kCal',
              style: themeData.textTheme.bodyLarge,
              textAlign: TextAlign.end,
            ),
          ),
        ],
      ),
      TableRow(
        children: [
          TableCell(
            child: Text(
              'Vitamin A:',
              style: themeData.textTheme.bodyLarge,
            ),
          ),
          TableCell(
            child: Text(veggie.vitaminAPercentage.toString()),
          ),
        ],
      ),
      TableRow(
        children: [
          TableCell(
            child: Text(
              'Vitamin C:',
              style: themeData.textTheme.bodyLarge,
            ),
          ),
          TableCell(
            child: Text(veggie.vitaminCPercentage.toString()),
          ),
        ],
      ),
    ],
  ),
```

##### 5.quick app create
```
Scaffold(
      body: <Widget>[
        _buildVeggieList(dateString),
        FavoritesScreen(),
        SearchScreen(),
        SettingScreen(),
      ][currentPageIndex],
      bottomNavigationBar: NavigationBar(
        onDestinationSelected: (int index) {
          setState(() {
            currentPageIndex = index;
          });
        },
        selectedIndex: currentPageIndex,
        destinations: const <Widget>[
          NavigationDestination(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          NavigationDestination(
            selectedIcon: Icon(Icons.bookmark),
            icon: Icon(Icons.bookmark_border),
            label: 'My Garden',
          ),
          NavigationDestination(
            icon: Icon(Icons.search),
            label: 'Search',
          ),
          NavigationDestination(
            icon: Icon(Icons.settings),
            label: 'Settings',
          ),
        ],
      ),
      appBar: currentPageIndex == 1 ? AppBar(title: Text("My Garden")) : null,
    );
 ```

##### 6.search example
```
import 'package:flutter/material.dart';
import 'package:veggieseasons_adaptive/data/veggie.dart';
import 'package:veggieseasons_adaptive/data/veggie_data.dart';
import 'package:veggieseasons_adaptive/widgets/veggie_headline.dart';

class SearchScreen extends StatefulWidget {
  const SearchScreen({super.key});

  @override
  State<SearchScreen> createState() => _SearchScreenState();
}

class _SearchScreenState extends State<SearchScreen> {
  late TextEditingController _controller;
  late String terms;

  @override
  void initState() {
    super.initState();
    _controller = TextEditingController();
    terms = "";
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _onTextChanged(String value) {
    setState(() => terms = value);
  }

  Widget _createSearchBox() {
    return Padding(
      padding: const EdgeInsets.all(8),
      child: TextField(
        controller: _controller,
        autofocus: true,
        decoration: InputDecoration(
          prefixIcon: Icon(Icons.search),
          border: OutlineInputBorder(),
          hintText: 'Search',
        ),
        onChanged: _onTextChanged,
      ),
    );
  }

  List<Veggie> _searchVeggies(String terms) => veggies
      .where((v) => v.name.toLowerCase().contains(terms.toLowerCase()))
      .toList();

  Widget _buildSearchResults(List<Veggie> veggies) {
    if (veggies.isEmpty) {
      return Center(
        child: Padding(
          padding: const EdgeInsets.symmetric(horizontal: 24),
          child: Text(
            'No veggies matching your search terms were found.',
            style: Theme.of(context).textTheme.bodyLarge,
          ),
        ),
      );
    }

    return ListView.builder(
      restorationId: 'list',
      itemCount: veggies.length,
      itemBuilder: (context, i) {
        return Padding(
          padding: const EdgeInsets.only(left: 16, right: 16, bottom: 24),
          child: VeggieHeadline(veggies[i]),
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: Column(
        children: [
          _createSearchBox(),
          Expanded(
            child: _buildSearchResults(
                // if the search team is not empty, filter results.
                terms.isNotEmpty ? _searchVeggies(terms) : veggies),
          ),
        ],
      ),
    );
  }
}

```
