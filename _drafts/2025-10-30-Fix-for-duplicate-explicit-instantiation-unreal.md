
Write:

__attribute__((exclude_from_explicit_instantiation))

before:

UFUNCTION()
TInstancedStruct<FType> FunctionName();

__attribute__((exclude_from_explicit_instantiation))
UFUNCTION()
TInstancedStruct<FType> FunctionName();