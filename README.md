# tinyify-crash
> reproducing an issue with tinyify run against plotly.js

To reproduce the issue:

```
npm install
npm run bundle
```

which prints:

```
> tinyify-crash@1.0.0 bundle /home/frederik/projects/tinyify-crash
> browserify main.js -o bundle.js -p tinyify


<--- Last few GCs --->

[9631:0x36f27f0]    56924 ms: Mark-sweep 1345.1 (1440.5) -> 1344.3 (1456.5) MB, 1270.8 / 0.0 ms  (average mu = 0.103, current mu = 0.006) allocation failure scavenge might not succeed
[9631:0x36f27f0]    58143 ms: Mark-sweep 1344.3 (1456.5) -> 1344.3 (1456.5) MB, 1218.0 / 0.0 ms  (average mu = 0.053, current mu = 0.000) allocation failure GC in old space requested


<--- JS stacktrace --->

==== JS stack trace =========================================

    0: ExitFrame [pc: 0x292a09adc01d]
Security context: 0x37fbe811e549 <JSObject>
    1: addHelpers [0x1401217d54e9] [/home/frederik/projects/tinyify-crash/node_modules/transform-ast/index.js:~87] [pc=0x292a0a5a7f5a](this=0x2f3e1e21aa09 <JSGlobal Object>,node=0x071fba8d11c1 <Node map $
 0x213b1d12dc9>)
    2: enter [0x208cdc2350f9] [/home/frederik/projects/tinyify-crash/node_modules/transform-ast/index.js:~33] [pc=0x292a09e5713a](this=0x2f...

FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory
 1: 0x8c02c0 node::Abort() [node]
 2: 0x8c030c  [node]
 3: 0xad15de v8::Utils::ReportOOMFailure(v8::internal::Isolate*, char const*, bool) [node]
 4: 0xad1814 v8::internal::V8::FatalProcessOutOfMemory(v8::internal::Isolate*, char const*, bool) [node]
 5: 0xebe752  [node]
 6: 0xebe858 v8::internal::Heap::CheckIneffectiveMarkCompact(unsigned long, double) [node]
 7: 0xeca982 v8::internal::Heap::PerformGarbageCollection(v8::internal::GarbageCollector, v8::GCCallbackFlags) [node]
 8: 0xecb2b4 v8::internal::Heap::CollectGarbage(v8::internal::AllocationSpace, v8::internal::GarbageCollectionReason, v8::GCCallbackFlags) [node]
 9: 0xecdf21 v8::internal::Heap::AllocateRawWithRetryOrFail(int, v8::internal::AllocationSpace, v8::internal::AllocationAlignment) [node]
10: 0xe96236  [node]
11: 0xea8a87 v8::internal::Factory::NewLoadHandler(int) [node]
12: 0xf2828e v8::internal::LoadHandler::LoadFullChain(v8::internal::Isolate*, v8::internal::Handle<v8::internal::Map>, v8::internal::Handle<v8::internal::Object>, v8::internal::Handle<v8::internal::Smi>) 
[node]
13: 0xf3663d v8::internal::LoadIC::UpdateCaches(v8::internal::LookupIterator*) [node]
14: 0xf36b6c v8::internal::LoadIC::Load(v8::internal::Handle<v8::internal::Object>, v8::internal::Handle<v8::internal::Name>) [node]
15: 0xf3b4a5 v8::internal::Runtime_LoadIC_Miss(int, v8::internal::Object**, v8::internal::Isolate*) [node]
16: 0x292a09adc01d 
Aborted (core dumped)
```

Bundling without the plugin does work:

```
npm run bundle:noplugin
```
