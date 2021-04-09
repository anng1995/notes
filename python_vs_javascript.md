# Python vs Node.js

## Node.js

    -Event-driven and asynchronous nature
    -Non-blocking i/o model
    -Lightweight and efficient
    -Perfect for data-intenseive real-time applicaitons for distrubted devices
    -Not meant to be used for CPU-bound tasks
    -Can use worker-threads to help
        -Runs in the same process and uses a message queue to communicate to
        main thread

## Flask

    -Use gunicorn to create more processes to has handle different requests

## Node.js vs Python

    -Nodejs is faster due to event-based architecture and sigle moduel caching
    -Python is not event driven, slow due to single flow
