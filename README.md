# Collection Views 

> Success is the good fortune that comes from aspiration, desperation, perspiration, and inspiration. -[Evan Esar](https://en.wikipedia.org/wiki/Evan_Esar)

## Overview

In this lesson we'll introduce you to collection views.

## Learning Objectives

* Present a data set to the user in an organized way using collection views
* Describe when to use collection views in their iOS apps
* List the various components of collection views: The view, controller, cells, and data source
* Explain how a collection view creates and uses cells to show data
* Explain reusable collection view cells and how they are created and dequeued

## What Are Collection Views?

_Collection views_ are one of the fundamental views that ship with Cocoa Touch and iOS. They provide a more flexible layout to present an ordered set of data items. Collection views are primarily used to present items in a gridlike arrangement but they are capable of much more. They are often used with apps that present a lot of data to the user. They are very similar to collection views but they allow for even more customization and can even scroll horizontally.

Collection views are made up of _cells_. Each cell represents a single piece of data in a larger data set. You can define how to draw a single cell, and the collection view will take care of the work of drawing many cells and scrolling them.

Collection views are backed by a _data source_. The data source provides the pieces of data that will be drawn into each individual cell of the collection.

## Fundamentals of Collection Views

As you've probably guessed by now, a collection view is a type of _view_ that is provided by Cocoa Touch. It is an instance of the class `UICollectionView`, which is a _subclass_ of `UIView`. This means that it provides all the functionality of a basic view (`UIView`), along with some functionality of its own.

Namely, a collection view knows how to lay out data in _items_. (Think rows.) It gets its data from a _source_ and draws each piece of that data set into a _cell_. Collection views allow users to scroll through the entire data set, displaying a cell for each piece of data, and can even allow the user to _select_ a cell to bring up more details for that piece of data.

### Powering Collection Views

Collection views get their data from a _data source_. A data source is typically another class that implements the [`UICollectionViewDataSource`](https://developer.apple.com/reference/uikit/uicollectionviewdatasource) protocol, which means it provides a number of methods that the collection view can call to retrieve individual pieces of data from the larger data set. Most of the methods described in the protocol are optional, but there are two that data sources are required to implement: `collectionView(_:cellForItemAt:)` and `collectionView(_:numberOfItemsInSection:)`.

Basically, when you set up a collection view, you can designate an instance of a given class as the view's data source. The view will then call methods on the data source, asking it questions like "How many items of data are in your data set?" and "What piece of data is on the 20th item of the collection?"

### Drawing Cells

Collection views comprise _cells_. Each cell is a single piece of data. A collection view cell is also a _view_ which is configured to display a given piece of data.

Unlike TableViews, CollectionViews do not come with basic cells that are preconfigured with labels.

## Working With Collection Views

An example of an app using a collection view has been created for you. Open up the `collectionViewFun.xcodeproj` file included in this repo. Select `Main.storyboard` from the file viewer to show the app's UI.

In `Main.storyboard`, you should see a single scene that includes a collection view. It looks like this:

![collection view in Xcode](https://s3.amazonaws.com/learn-verified/basicCollectionViewStoryboard.png)

Interface Builder only shows a basic example of how the collection view will look. Notice that the collection view takes up the entire UI of the application. There is also a _prototype cell_ drawn at the top of the collection view. This gives you a basic idea of what the cell will look like, but it is not yet populated with any data. The collection view itself has no data in Interface Builder, either, so it mostly looks blank.

Try building and running the app. In the simulator, you'll see this:

![collection view in simulator](https://s3.amazonaws.com/learn-verified/collectionViewWithCells.png)

When the app is actually run, it is populated with data (in this case, some random cells with random colors). How does the app get populated with data? Is it magic?

You're about to find out!

The "magic" happens in the app's view controller. Open up `ViewController.swift` to see the code that powers the view controller. In particular, take a look at the section marked `Collection View DataSource`. You'll see that there are currently three methods defined:

```swift
    // MARK: - Collection View DataSource

    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return 100
    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "basicCell", for: indexPath)

        cell.backgroundColor = UIColor.getRandomColor()
        return cell
    }
```


Believe it or not, these are all the methods that are required to power a collection view. The collection view can use these methods to get its data set, and will do all the rest of the work itself—drawing the cells, allow the user to scroll through the collection view, and so on.

Let's take a look at these methods in more detail.

### collection View Methods

The first method is `collectionView(_:numberOfItemsInSection:)`. This method gets passed an `Int` representing a particular section of the collection view, and returns the number of _items_ present in that section. Since you're working with collection views with only one section in this example, `numberOfItemsInSection` will always be 0 (since sections are 0-indexed, like arrays).

The relevant bit of this method is the single line that returns the number of items in the section. In this example app, you are creating a collection view that simply shows different colored cells in a collection, so this method simply returns an `Int` that you choose. Easy, right?

The final method, `collectionView(_:cellForItemAt:)`, is a bit more complicated. There's a lot going on in this method, so let's break it down bit by bit.

#### Getting a Cell

`collectionView(_:cellForItemAt:)` does something very important: It returns an actual _item_ for use in the collection view. The "magic" is that, given a cell, the collection view will know how to draw it in amongst all the other cells. Neat, huh?

First, `collectionView(_:cellForItemAt:)` gets passed an `indexPath` parameter which lets the method know exactly which cell is being requested. This parameter is of type `NSIndexPath`, which is a structure that contains both the _section_ and _item_ of the cell being drawn. In this example application, all that matters is setting the background color which we have done for you.

Let's talk about reusable cells.

The method's first line of code is this:

```swift
let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "basicCell", for: indexPath)
```

This is very similar to what you should have seen with a tableView. Using what you have learnt with table views apply the same thinking to collection views. Experiment with the collection view in the storyboard and in code to get different results.

As an extra activity you should try changing the sizes of cells and the spacing between them. One of the challenges with making collection views is getting the spacing and sizing just right. Go to `main.storyboard`, then click on the `UICollectionView` and finally click on the Size Inspector to see the fields where you can change size and spacing.

![collection view size inspector](https://s3.amazonaws.com/learn-verified/collectionViewCellSize.png)

In future lessons, you'll see how to build your own iOS app powered by a collection view yourself. For now, though, you've hopefully gained insight on how collection views in iOS work.

<a href='https://learn.co/lessons/collectionView' data-visibility='hidden'>View this lesson on Learn.co</a>
