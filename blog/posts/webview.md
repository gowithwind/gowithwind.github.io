#android webview open file not working  problem
>20150319

#why not working
because android 's openfilechooser method is not public, and even in 4.4 missed.I feel worrying about it.

#my solution

addeventlistner for ->when click file input control ->

android native call ->start a intent to choose photo->read file and base64 it ->

call webview ->dispatch a event to processimage

#why not phonegap
I just wanna do sth simple ,a few  native functions(such as file read/write,share) do it self. 
