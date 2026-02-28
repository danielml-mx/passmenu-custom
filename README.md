# passmenu-custom

`passmenu` is a dmenu interface to the [`pass`](https://www.passwordstore.org/) password manager. 

`passmenu-custom` extends the functionality of the original with integrated OTP handling and additional options.

## Installation

Clone the repo and copy the script to `~/.local/bin/`. Make sure said path is in $PATH.

## Usage

Like the original, calling `passmenu-custom` will display a dmenu prompt listing all available passwords. All arguments will be passed to dmenu, save for:

### `--type`
Types the password instead of copying it to the clipboard.

### `--delay N`
Delay between each typed keystroke. Requires `--type`. Default is 12ms.

### `--hitenter`
Automatically send a newline after the password is typed. Requires `--type`.

## OTP handling
`passmenu-custom` automatically generates and copies an OTP code for those passwords that have an `otpauth://` URI appended to them. Unlike other implementations, `passmenu-custom` does not use `pass grep`, something which would make the script unacceptably slow, but rather detects if the selected password contains an appended URI and acts accordingly.

Finally, like the original `passmenu`, entries that only include an OTP URI and no password will not be handled properly. Additionally, OTP code generation will only work when the `--type` flag is passed, since otherwise the copied code would override the copied password. For both of these use cases, a simple `pass otp [password]` can be used.

## Examples

Call `passmenu-custom` to type with no delays:
                    
    passmenu-custom --type --delay 0

Call `passmenu-custom` to type with no delays and send a newline afterwards:
                    
    passmenu-custom --type --delay 0 --hitenter
    
Call `passmenu-custom` to type with no delays and send a newline afterwards, passing some arguments to dmenu:

    passmenu-custom --type --delay 0 --hitenter -c -l 20 -p '🔑: '

Call `passmenu-custom` to type with no delays and send a newline afterwards, passing some arguments to dmenu, and generating a notification with the copied OTP code:

    passmenu-custom --type --delay 0 --hitenter -c -l 20 -p '🔑: ' && notify-send -a "pass" "OTP" "OTP code copied to clipboard: $(xclip -selection clipboard -o)"

