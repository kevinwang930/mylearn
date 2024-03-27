# 1. launch

```plantuml
activate main.c
main.c --> java.c:JLI_launch
activate java.c
java.c --> java_md.c: loadJavaVm
java_md.c --> jni.cpp: JNI_CreateJavaVM
java.c --> java.c:jvmInit
activate java.c
java.c --> java.c:ContinueInNewThread
java.c --> java.c:CallJavaMainInNewThread
activate java.c
java.c --> java.c:JavaMain

```



# 2. Thread

```mermaid
flowchart LR
threadCreate --> threadPrepare --> threadStart

```