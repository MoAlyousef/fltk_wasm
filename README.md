# fltk_wasm demo

To build:
```
$ git clone https://github.com/MoAlyousef/fltk_wasm
$ cd fltk_wasm
$ git submodule update --init
$ cd fltk
$ git apply ../wasm.patch
$ cd ..
$ emcmake cmake -Bbin && cmake --build bin --parallel
```

emscripten_set_main_loop_arg() needs to be used to run the main loop:
```cpp
#include <FL/Fl.H>
#include <FL/Fl_Window.H>
#include <FL/Fl_Button.H>
#include <FL/Fl_Box.H>
#include <emscripten.h>

void main_loop(void *) {
    Fl::wait();
}

void cb(Fl_Widget *w, void *data) {
    ((Fl_Box*)data)->color(FL_RED);
    Fl::redraw();
}

int main() {
    auto win = new Fl_Window(600, 400);
    auto box = new Fl_Box(40, 40, 520, 220, "Hello from FLTK 1.4");
    box->box(FL_SHADOW_BOX);
    box->labelsize(16);
    auto but = new Fl_Button(260, 300, 80, 40, "Click me");
    win->end();
    win->show();
    but->callback(cb, box);
    emscripten_set_main_loop_arg(main_loop, NULL, 0, true);
}
```

The resulting binary needs a server to be run, you can use emrun, python -m https.server ...etc.

The demo can be accessed here:
https://moalyousef.github.io/fltk_wasm/