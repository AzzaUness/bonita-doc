
1.7.2.7 Custom widgets
======================

If the [standard widgets](/widgets-0) do not meet your needs, you can create a custom widget to match exactly what you want to.
After creation, a custom widget is available in the palette for designing pages, forms or fragments.

[Description](#description)\
[Custom widget properties](#custom-widget-properties)\
[Using an external AngularJS directive or service in a custom widget](#using-external-angularjs-directive-or-service-in-a-custom-widget)\
[Translation filter](#custom-widget-translation)\
[Advanced Example](#advanced-example)

Description
-----------

Custom widgets are composed of:

-   A description that briefly describes the widget
-   Properties exposed by the widget
-   A template that describe the widget HTML markup
-   A controller that describe the widget logic
-   Some assets if the widget uses external JavaScript libraries, CSS definitions, or images
-   AngularJS modules declaration if the widget uses services or directives that are not built in to the AngularJS framework

Custom widget implementation is based on the AngularJS framework.
When you open the custom widget editor to create a new custom widget, a simple example is displayed to show the basic uses of these concepts.

For more information, see the AngularJS guide on
[templates](https://docs.angularjs.org/guide/templates) and
[controllers](https://docs.angularjs.org/guide/controller).

For now, you cannot implement custom widget containers.

Custom widget properties
------------------------

Properties are variables that can be set in the properties panel when adding a custom widget to a page, form, or fragment.
A property is constant, dynamic, a bidirectional bond, or an interpolation. You can chose between these treatments when you edit the [widget property](/widget-properties#value-types).
Use properties when you want the person that uses your widget to customize widget appearance or behavior. In a custom widget, a property can be used either in the template or in the controller.

A property has a *name* which can be referenced in the controller or the template, a *label* which will be printed in the properties panel in an editor, a *default* value, and a type.
There are several property types:

-   **text** user specifies the property value with some text
-   **choice** user chooses the property value from a list that you define
-   **html** user specifies the property value with some text that can contain basic HTML tags
-   **integer** property value is an integer
-   **boolean** property value is a Boolean
-   **collection** property value is an array

Each of these types will allow the user of the custom widget to bind the property value to a variable.

Using an external AngularJS directive or service in a custom widget {#using-external-angularjs-directive-or-service-in-a-custom-widget}
-------------------------------------------------------------------

Many Angular libraries are available on the Internet and can be used in a custom widget.
To do so, download the library you want to use and add it to your custom widget as a JavaScript asset.
The downloaded library will specify a module to add to your application: add this module to the custom widget definition in the **Required Angular modules** field.

Now you can use the imported library's directives in the custom widget templates and services in the controller by injecting them in function arguments (example: `function($scope, myImportedService)`).

Translation filter {#custom-widget-translation}
------------------

The [Multi-language pages](multi-language-pages) explains how to manage localization (l10) inside developped pages. During page loading, the l10n strings are loaded to the l10n mechanism.

You can use this mechanism in the Widgets to process content to translate. There are two ways to use the localization: either the *uiTranslate* angular filter or *ui-translate* directive:

-   Use the *uiTranslate* filter in custom widgets for dynamic content to be translated
-   Use the *ui-translate* directive in custom widgets for static content to be translated

#### For instance, with a custom link widget :

    <div>
        <a title="{{ properties.title | uiTranslate }}" ui-translate>Submit</a>
    </div>

The link text which is static is translated using the *ui-translate* directive. The link text title held inside the *properties.title* variable is translated using the *uiTranslate* filter.

Advanced example
----------------

This section explains how to use the *carousel* and *slide* directives from the `ui.bootstrap` library.

Following the [`ui.bootstrap` documentation](https://angular-ui.github.io/bootstrap/#/getting_started),
[download `ui-bootstrap-tpls.min.js`](https://angular-ui.github.io/bootstrap/#/carousel) and declare the module `ui.bootstrap` as a dependency of the application.
Specifically, to use these directive in a custom widget, you need to:

1.  Download `ui-bootstrap-tpls.min.js`.
2.  Add it as local JavaScript asset to the custom widget.
3.  Declare the *ui.bootstrap* module in the *Required angular modules* field.

You can now use the *carousel* and *slide* directives in your template. Here is a simple example of the template and controller for a carousel.

Template :

        <carousel>
          <slide ng-repeat="slide in slides" >
            <img ng-src="{{slide.url}}" style="margin:auto;" alt="{{slide.alt | uiTranslate}}">
          </slide>
        </carousel>

Controller:

        function ($scope) {
          $scope.slides = [
            { url: 'http://placekitten.com/601/300', alt:'kitty looking down' },
            { url: 'http://placekitten.com/602/300', alt:'kitty in hand' },
            { url: 'http://placekitten.com/603/300', alt:'kitty outside' },
            { url: 'http://placekitten.com/604/300', alt:'4 kitten in basket' }
           ];
        }

