NSString-URLEncoding
====================

A (very) small category to Percent Encoding a given string, also known as URL encoding.

Useful for encoding a string, eg: Markdown, then parse it in a `NSURL`, so you can open it in URL scheme in your app.

*If you are new about this, please see: http://en.wikipedia.org/wiki/Percent-encoding.*

## What's inside ##


```objective-c
- (NSString *)urlEncodeUsingEncoding:(NSStringEncoding)encoding 
{
  return (NSString *)CFBridgingRelease(CFURLCreateStringByAddingPercentEscapes(NULL,
                                                               (CFStringRef)self,
                                                               NULL,
                                                               (CFStringRef)@"!*'\"();:@&=+$,/?%#[]% ",
                                                               CFStringConvertNSStringEncodingToEncoding(encoding)));
}
```

yeah, that's it!

## Include ##

Simply include `NSString+URLEncoding.h` in your class in which you want to use the category. 

Or you can include `#import "NSString+URLEncoding.h"` in your prefix header file, this will tell the compiler to precompile header in every class in your project, so no need to include seperately.

## Usage ##

```objective-c

- (void)viewDidLoad 
{
  [super viewDidLoad];
    
  NSString *someThoughts           = @"Some Random Thoughts";
  NSString *myAwesomeString        = @"#What a day#, *I* feel **inspired** and ###motivated###!"; // eg: a markdown string

  NSString *encodedTitle           = [someThoughts stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
  NSString *myAwesomeEncodedString = [titleString stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];

  UIButton *someButton                    = [[UIButton alloc] initWithFrame:CGRectMake(0, 0, 44, 44)];
  [someButton setImage:[UIImage imageNamed:@"some-icon"] forState:UIControlStateNormal];
  [someButton setShowsTouchWhenHighlighted:TRUE];
  [someButton addTarget:self
                  action:@selector(openURLScheme:)
        forControlEvents:UIControlEventTouchDown];
  UIBarButtonItem *someButtonItem         = [[UIBarButtonItem alloc] initWithCustomView:someButton];
  self.navigationItem.rightBarButtonItems = @[someButtonItem];
}
```

In example, we want to parse UITextView's text which is Markdown to Fanstatical.

Fantastical URL scheme:
`fantastical://parse?sentence=sentence&notes=[your note].`

```objective-c
- (IBAction)openURLScheme:(id)sender
{
  NSURL *urlScheme = [NSURL urlWithString:[NSString stringWithFormat:@"fantastical://parse?sentence=%@&notes=%@", encodedTitle, myAwesomeEncodedString]];
  if ([UIApplication sharedApplication] canOpenURL:[NSURL urlWithString:@"fantastical://"]) {
    [[UIApplication sharedApplication] openURL:urlScheme];
  }

  NSLog(@"urlScheme: %@", urlScheme); 
  // urlScheme: fantastical%3A%2F%2Ftask%2Fcreate%3Ftitle%3DSome%20Random%20Thoughts%26text%3D%23What%20a%20day%23%2C%20*I*%20feel%20**inspired**%20and%20%23%23%23motivated%23%23%23!
}

```

## License ##

The MIT License (MIT)

Copyright (c) 2013 Vinh Nguyen

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

