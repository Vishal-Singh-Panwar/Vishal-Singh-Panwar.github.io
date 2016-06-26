---
layout: post
title: Dropdown for iOS! [In Progress]
---

[`VSDropdown`](https://github.com/Vishal-Singh-Panwar/VSDropdown) is an iOS drop-in class which can be used to show menu item below/above UIButton. This class adapts appearance of the button for which it is shown, which you can customise, and presents itself  with appropriate frame and direction. Irrespective of the button's hierarchy in the Window, the Dropdown takes touches everywhere on the screen. It dismisses itself when tapped outside its bounds.



Usage
==========

```
[_dropdown setupDropdownForView:_myButton];

[_dropdown reloadDropdownWithContents:@[@"Hello World",@"Dropdown test",@"Bla Bla bla.."] andSelectedItems:@[_myButton.titleLabel.text]];

```
If you have an Array containing custom modal classes, for eg. `countries` array containing `Country` objects. You want to show value of `name` property of all `Country` objects in dropdown list , you can use below method. `name` should be of `NSString` type.

```
[_dropdown reloadDropdownWithContents:self.countries keyPath:@"name" selectedItems:@[_myButton.titleLabel.text]];

```
##Other
`adoptParentTheme` property can be used when the button has solid background color. When this is YES, the dropdown draws itslef with a gradient color which matches with the button's background color. 

You can tweak the componets of background color using below functions:

```
-(void)setupDropdownForView:(UIView *)view direction:(Dropdown_Direction)direction withBaseColor:(UIColor *)baseColor scale:(float)scale;
   
-(void)setupDropdownForView:(UIView *)view direction:(Dropdown_Direction)direction withTopColor:(UIColor *)topColor bottomColor:(UIColor *)bottomColor scale:(float)scale;
```

About Sample
==========

In the [sample](https://github.com/Vishal-Singh-Panwar/VSDropdown), there are buttons of different background colors, sizes and fonts. Note that only one `VSDropdown` instance is used for all the button. Whenever a `setupDropDownForView:` message is called on `VSDropdown` instance, it removes itslef from its previous superview, if any, and draws itslef again for the  UIButton passed in the argument.



How it looks?
==========
![Github Screenshots](https://raw.githubusercontent.com/Vishal-Singh-Panwar/VSDropdown/master/Screenshots/Combined.png "Combined") 

