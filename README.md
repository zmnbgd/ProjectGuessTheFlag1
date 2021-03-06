# ProjectGuessFlag1
Paul Hudson 100DaysSwiftUI


DAY 20


Today you have six topics to work through, and you’ll meet VStack, Image, LinearGradient, and more.
This project is still going to be nice and easy, but gives me chance to introduce you to whole range of new SwiftUI functionality: stacks, buttons, images, alerts, asset catalogs, and more. 


Using stacks to arrange views

When we return some View for our body, SwiftUI expects to receive back some kind of view that can be displayed on the screen. That might be a navigation view, a form, a text view, a picker, or something else entirely, but it must conform to the View protocol so that it can be drawn on the screen.

If we want to return multiple things we have various options, but three are particularly useful. They are HStack, VStack, and ZStack, which handle horizontal, vertical, and, er, zepth.

So, for VStack – a vertical stack of views – the two text views would be placed one above the other, like this:

var body: some View {
    VStack {
        Text("Hello, world!")
        Text("This is inside a stack")
    }
}

By default VStack places some automatic amount of spacing between the two views, but we can control the spacing by providing a parameter when we create the stack, like this:

VStack(spacing: 20) {
    Text("Hello, world!")
    Text("This is inside a stack")
}

Just like SwiftUI’s other views, VStack can have a maximum of 10 children – if you want to add more, you should wrap them inside a Group.

By default, VStack aligns its views so they are centered, but you can control that with its alignment property. For example, this aligns the text views to their leading edge, which in a left-to-right language such as English will cause them to be aligned to the left:

VStack(alignment: .leading) {
    Text("Hello, world!")
    Text("This is inside a stack")
}

Alongside VStack we have HStack for arranging things horizontally. This has the same syntax as VStack, including the ability to add spacing and alignment:

HStack(spacing: 20) {
    Text("Hello, world!")
    Text("This is inside a stack")
}

Vertical and horizontal stacks automatically fit their content, and prefer to align themselves to the center of the available space. If you want to change that you can use one or more Spacer views to push the contents of your stack to one side. These automatically take up all remaining space, so if you add one at the end a VStack it will push all your views to the top of the screen:

If you add more than one spacer they will divide the available space between them. So, for example we could have one third of the space at the top and two thirds at the bottom, like this:

VStack {
    Spacer()
    Text("First")
    Text("Second")
    Text("Third")
    Spacer()
    Spacer()
}

We also have ZStack for arranging things by depth – it makes views that overlap.

ZStack doesn’t have the concept of spacing because the views overlap, but it does have alignment. So, if you have one large thing and one small thing inside your ZStack, you can make both views align to the top like this: ZStack(alignment: .top) {.

ZStack draws its contents from top to bottom, back to front.



Colors and frames

SwiftUI gives us a range of functionality to render colors, and manages to be both simple and powerful – a difficult combination.

If we want to put something behind the text, we need to place it above it in the ZStack. But what if we wanted to put some red behind there – how would we do that?

ZStack {
    Text("Your content")
}
.background(.red)

If you want to fill in red the whole area behind the text, you should place the color into the ZStack – treat it as a whole view, all by itself:

    ZStack {
        Color.red
        Text("Your content")
    }

In fact, Color.red is a view in its own right, which is why it can be used like shapes and text.


Tip: When we were using the background() modifier, SwiftUI was able to figure out that .red actually meant Color.red. When we’re using the color as a free-standing view Swift has no context to help it figure out what .red means so we need to be specific that we mean Color.red.


Colors automatically take up all the space available, but you can also use the frame() modifier to ask for specific sizes. For example, we could ask for a 200x200 red square like this:


Color.red
    .frame(width: 200, height: 200)

You can also specify minimum and maximum widths and heights.

Color.red
    .frame(minWidth: 200, maxWidth: .infinity, maxHeight: 200)


If you need something specific, you can create custom colors by passing in values between 0 and 1 for red, green, and blue, like this:

Color(red: 1, green: 0.8, blue: 0)

If you want your content to go under the safe area, you can use the .ignoresSafeArea() modifier to specify which screen edges you want to run up to, or specify nothing to automatically go edge to edge.

ZStack {
    Color.red
    Text("Your content")
}
.ignoresSafeArea() 



Gradients

SwiftUI gives us three kinds of gradients to work with, and like colors they are also views that can be drawn in our UI.
Gradients are made up of several components:
* An array of colors to show
* Size and direction information
* The type of gradient to use

For example, a linear gradient goes in one direction, so we provide it with a start and end point like this:

LinearGradient(gradient: Gradient(colors: [.white, .black]), startPoint: .top, endPoint: .bottom)

As an alternative, radial gradients move outward in a circle shape, so instead of specifying a direction we specify a start and end radius – how far from the center of the circle the color should start and stop changing. For example:

RadialGradient(gradient: Gradient(colors: [.blue, .black]), center: .center, startRadius: 20, endRadius: 200)

The last gradient type is called an angular gradient, although you might have heard it referred to elsewhere as a conic or conical gradient. This cycles colors around a circle rather than radiating outward, and can create some beautiful effects.

For example, this cycles through a range of colors in a single gradient, centered on the middle of the gradient:

AngularGradient(gradient: Gradient(colors: [.red, .yellow, .green, .blue, .purple, .red]), center: .center)

Gradients

SwiftUI gives us three kinds of gradients to work with, and like colors they are also views that can be drawn in our UI.
Gradients are made up of several components:
* An array of colors to show
* Size and direction information
* The type of gradient to use

For example, a linear gradient goes in one direction, so we provide it with a start and end point like this:

LinearGradient(gradient: Gradient(colors: [.white, .black]), startPoint: .top, endPoint: .bottom)

As an alternative, radial gradients move outward in a circle shape, so instead of specifying a direction we specify a start and end radius – how far from the center of the circle the color should start and stop changing. For example:

RadialGradient(gradient: Gradient(colors: [.blue, .black]), center: .center, startRadius: 20, endRadius: 200)

The last gradient type is called an angular gradient, although you might have heard it referred to elsewhere as a conic or conical gradient. This cycles colors around a circle rather than radiating outward, and can create some beautiful effects.

For example, this cycles through a range of colors in a single gradient, centered on the middle of the gradient:

AngularGradient(gradient: Gradient(colors: [.red, .yellow, .green, .blue, .purple, .red]), center: .center)


Buttons and images

The simplest way to make a button is one we’ve looked at previously: when it just contains some text you pass in the title of the button, along with a closure that should be run when the button is tapped:

Button("Delete selection") {
    print("Now deleting…")
}

That could be any function rather than just a closure.

struct ContentView: View {
    var body: some View {
        Button("Delete selection", action: executeDelete)
    }

    func executeDelete() {
        print("Now deleting…")
    }
}

SwiftUI has a dedicated Image type for handling pictures in your apps, and there are three main ways you will create them:

* Image("pencil") will load an image called “Pencil” that you have added to your project.
* Image(decorative: "pencil") will load the same image, but won’t read it out for users who have enabled the screen reader. This is useful for images that don’t convey additional important information.
* Image(systemName: "pencil") will load the pencil icon that is built into iOS. This uses Apple’s SF Symbols icon collection, and you can search for icons you like – download Apple’s free SF Symbols app from the web to see the full set.

If you want both text and image at the same time, SwiftUI has a dedicated type called Label.


Showing alert messages

If something important happens, a common way of notifying the user is using an alert – a pop up window that contains a title, message, and one or two buttons depending on what you need.

But think about it: when should an alert be shown and how? Views are a function of our program state, and alerts aren’t an exception to that. So, rather than saying “show the alert”, we instead create our alert and set the conditions under which it should be shown.

A basic SwiftUI alert has a title and a button that dismisses it, but the more interesting part is how we present that alert: we don’t assign the alert to a variable then write something like myAlert.show(), because that would be back to the old “series of events” way of thinking.
Instead, we create some state that tracks whether our alert is showing, like this:

@State private var showingAlert = false

We then attach our alert somewhere to our user interface, telling it to use that state to determine whether the alert is presented or not. SwiftUI will watch showingAlert, and as soon as it becomes true it will show the alert.

struct ContentView: View {
    @State private var showingAlert = false

    var body: some View {
        Button("Show Alert") {
            showingAlert = true
        }
        .alert("Important message", isPresented: $showingAlert) {
            Button("OK") { }
        }
    }
}

That attaches the alert to the button, but honestly it doesn’t matter where the alert() modifier is used – all we’re doing is saying that an alert exists and is shown when showingAlert is true.
Take a close look at the alert() modifier:

alert("Important message", isPresented: $showingAlert)

The first part is the alert title, which is straightforward enough, but there’s also another two-way data binding because SwiftUI will automatically set showingAlert back to false when the alert is dismissed.

Now look at the button:

Button("OK") { }

That’s an empty closure, meaning that we aren’t assigning any functionality to run when the button is pressed. That doesn’t matter, though, because any button inside an alert will automatically dismiss the alert – that closure is there to let us add any extra functionality beyond just dismissing the alert.

You can add more buttons to your alert, and this is a particularly good place to add roles to make sure it’s clear what each button does:

.alert("Important message", isPresented: $showingAlert) {
    Button("Delete", role: .destructive) { }
    Button("Cancel", role: .cancel) { }
}


![Screen ShotGuessTheFlag1](https://user-images.githubusercontent.com/32281586/166154936-f102845f-a55f-452c-83da-377e670e3c88.png)



