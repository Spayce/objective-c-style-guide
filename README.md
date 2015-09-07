# SPayCe Objective-C Style Guide

This style guide outlines the coding conventions of the iOS teams at Spayce.

## Introduction

Here are some of the documents from Apple that informed the style guide. If something isn’t mentioned here, it’s probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html) - A Great Resource (!)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

The intent of this guide is to bring our codebase one step closer in achieving the following:

1. Readability: The primary consumers of our codebase are our developers/ourselves/you! A uniform approach ensures that our code is readable and allows jumping into the codebase with little friction.
2. Maintainability: As time progresses, we will add/remove features and introduce/address issues. With a proactive approach to maintainability, many unintended side effects and artifacts of these changes can be avoided.
3. Reusability: It is often recommended to rely on a battle-tested piece of code rather than introducing a new implementation. This leads to keeping many principles in mind when introducing code (e.g. modularity, loose coupling, cohesion, and separation of concerns; [required reading: wiki](https://en.wikipedia.org/wiki/Code_reuse)).

## Table of Contents

* [Naming](#naming)
* [Spacing](#spacing)
* [Consts/Enums/Variables/Types](#constsenumsvariablestypes)
* [MVC](#mvc)
* [Comments](#comments)
* [General](#general)
* [Designated inits/inits/dealloc](#designated-initsinitsdealloc)
* [Conditionals](#conditionals)
* [Imports](#imports)
* [Delegates](#delegates)
* [Returns](#returns)

## Naming

* Do NOT abbreviate const/variable/method names. [Acceptable abbreviations here.](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html)
* Use 3-letter prefixes, not 2-letter prefixes. Use class- or application- or framework-level prefixes, as necessary
* Use camelCase for consts/variables/methods/everything, however use an all-capital prefix where appropriate
* Do NOT use the word ’and’ when declaring a method or in its parameter list, unless using it to separate distinct actions of the method
* Optional: When subclassing third-party code, you may prefix private methods in order to prevent an unintentional method override
* Qualify variables with their type if not a basic type and there is any possibility of type ambiguation
* Use ‘sharedInstance’ as the accessor for the shared instance of a singleton
* Naming notifications: Class + Did/Will + Action + Notification
* Avoid explicitly declaring instance variables, i.e. use public or private properties

## Spacing

* Use tabs, set to an equivalent of 4 spaces
* In headers, group similar methods together on sequential lines. Separate groups of methods with a single line
* Single empty line between methods in implementation files
* Space after the initial ‘-‘/‘+’, preceding asterisks in argument declarations, between parameters, and preceding the opening bracket (which is placed on the same line) e.g.:
- (NSString *)stringFromExampleObject:(NSObject *)exampleObject forDisplaySize:(DisplaySize)displaySize {
* Prepend the asterisk to the variable name, not its type. The only exception is when declaring a const string, i.e. NSString * const kPREVariableName = @“”;
* When using literals (@[], @{}), place one space after the opening bracket, one space before the closing bracket, and one space (or line) after the comma of each item. Do NOT using spaces inside of parenthesis
* If/Else bracketing - Place the opening bracket on the same line as the if/else. Spayce: Closing bracket on its own line. Mix: Closing bracket on the same line as the ‘else’
* Use #pragma mark - Description to section off cohesive sections of code
* @interface PFXClassName() <PFXClassDelegate>
* @property (nonatomic, weak) id<PFXClassDelegate> delegate;
* Do not use more than 1 consecutive empty line within a method.  Feel free to include 1-line breaks between groups of related statements for readability
* Use spaces between the operands of binary or ternary operators.  No spaces for unary operators.  No spaces before “;”.  No spaces after open paren or before close paren.  Use parentheses whenever order-of-operation is significant in the result -- e.g. no need when val1 + val2 + val3, but should use val1 + (val2 * val3).  Feel free to use parentheses in other contexts, for clarity

## Consts/Enums/Variables/Types

* Use NSInteger, CGFloat, NSTimerInterval, and other Apple-provided types when possible
* Use constants in when possible over random/unnamed/magic numbers
* Avoid using #define in order to declare constants
* Use NSUInteger for indexing
* Use enums when required to declare constants that are related to each other, e.g. day of the week
* Declare enums through typedef NS_ENUM(NSInteger, PFXEnumName) {};
* Reserve the ‘0’ enum value for an unknown/uninitialized value
* Use CGRectGetXXX in place of accessing a CGRect struct directly

## MVC

* If a view has more than trivial functionality, separate it into its own class
* A view controller (A) that presents view controller (B). A passes data to B via B’s public properties or public methods; B passes data to A via a delegate method, or a notification in the case of general information that may be relevant to non-presenters, e.g. model object updates, not anything that requires B to have specific knowledge of A’s class
* In most cases, the init method should be reserved for object instantiation/initialization, while size/position/frame-related operations should take place in the layoutSubviews method. Exceptions can include UITableView/UICollectionView cells, where reuse optimization may involve setting layout properties in init methods for views that remain static during the cell’s lifecycle
* In general, if model data is updated, this update should propagate through the app via a notification, i.e. using NSNotificationCenter
* If a data source is going to be reused across classes, create a separate class file for the data source
* If a controller has multiple data sources for similar UI elements (i.e. supporting 2 different tables in the same VC) separate the data sources out into separate class files rather than using conditional checks within  shared data source methods

## Comments

* Comments are acceptable, when keeping the following in mind that they are not compiled, thus they may not be maintained and kept up-to-date like actual code
* Comments should explain ‘why’ a piece of code does what it does. In most cases, the actual code should be readable and descriptive enough in order to mitigate the need for comments
* Keep comments short, typically using a double-forward-slash at the beginning of each comment

## General

* Use class-, application-, or framework-level prefixes in order to simulate namespaces, keeping code within the scope it is intended to be used
* In general, encapsulate/abstract calls to third-party libraries/frameworks into a single Manager/Proxy/Utility class. Exceptions involve cases where abstraction does not grant advantages, e.g. Google Maps iOS SDK, in which a map view abstraction would be useless and would only add an additional, and almost identical, layer between the view controller and the map view
* Do NOT introduce less-abstract data/models/information/details into a more-abstract class. For example, a framework-level class should not have knowledge of the inner workings of any application-level models/data
* Developers should utilize the same version of Xcode and update together, as updates are released. Warnings vary across various versions of Xcode

## Designated inits/inits/dealloc

* Designate a single init method as the initializer that all init methods should point to. Denote this designation using comments in the header. Subclasses should take note of overriding at least the designated initializer
* Reference the instance variable (_variableName) directly in the init and dealloc methods. Outside of these methods, reference the accessor, self.variableName
* Clean/clear/nil variables, as needed, in the same order they are declared in the anonymous extension (the @interface PFXClassName() {})

## Conditionals

* Do NOT compare against nil, unless the comparison dictates the flow of logic; however ensure you do not dereference NULL
* Use ‘if (variableName)’ and ‘if (!variableName)’
* You may split expression groups across different lines, placing the conditional operator (&&/||) at the end of the line
* Use brackets for ALL ‘if’ statements, regardless of if they’re “single-liner”s
* Use ternary operators if desired and if readability is not hindered

## Imports

* Use #import (over #include)
* Group #imports according to // Controllers, // Models, // Views, // Utilities, etc. Groups should be ordered alphabetically

## Delegates

* Delegates for a specific class should be declared in that class’s header file and name PFXClassName + Delegate
* Delegate properties should, more often than not (read: 99.9% of the time), be declared ‘weak’
* Delegate methods should return the delegate object as the first argument (or only object if there are no other arguments), similar to a UITableView delegate e.g.
  - (void)class:(PFXClassDelegate *)class didSendAMessage:(NSObject *)messageObj;
  - (void)classDidDoSomething:(PFXClassDelegate *)class;

## Returns

* Both multiple returns and single returns in any given method are acceptable
