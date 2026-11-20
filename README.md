# mmlanscan

mmlanscan helps an iOS app discover devices on the local network. It scans hosts, reports their IP addresses, and provides available hostname, MAC address, and vendor information.

## Features

- Scan hosts on a local network subnet.
- Report each discovered device through a delegate callback.
- Show IP addresses for discovered hosts.
- Resolve hostnames when one is available.
- Look up device vendors from MAC addresses with the bundled OUI mappings.
- Report scan progress and completion status.

MAC address and vendor detection depend on ARP access. On iOS 11 and later, those values may be unavailable.

## Installation

With CocoaPods, add the library to your `Podfile`:

```ruby
pod 'MMLanScan'
```

Then install the dependency:

```sh
pod install
```

Open the generated `.xcworkspace`, import the scanner header, and link the library with your iOS target.

For manual installation, clone the repository and add the `MMLanScan` source folder to your Xcode project:

```sh
git clone https://github.com/your-org/mmlanscan.git
```

## Usage

Create a scanner with a delegate, start the scan, and handle devices as they are found:

```objc
#import "MMLANScanner.h"

@interface ViewController () <MMLANScannerDelegate>
@property (nonatomic, strong) MMLANScanner *lanScanner;
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    self.lanScanner = [[MMLANScanner alloc] initWithDelegate:self];
    [self.lanScanner start];
}

- (void)lanScanDidFindNewDevice:(MMDevice *)device {
    NSLog(@"Found device: %@", device);
}

- (void)lanScanProgressPinged:(NSInteger)pingedHosts
                          from:(NSInteger)overallHosts {
    NSLog(@"Scan progress: %ld of %ld hosts",
          (long)pingedHosts,
          (long)overallHosts);
}

- (void)lanScanDidFinishScanningWithStatus:(MMLanScannerStatus)status {
    NSLog(@"LAN scan finished with status: %ld", (long)status);
}

- (void)lanScanDidFailedToScan {
    NSLog(@"LAN scan failed");
}

@end
```

Stop an active scan when the view or task no longer needs it:

```objc
[self.lanScanner stop];
```

## License

mmlanscan is released under the MIT License. This project is based on [mavris/MMLanScan](https://github.com/mavris/MMLanScan), and retains the required attribution under that license.
