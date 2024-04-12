---
title: POI Filtering Design
description: Point of Interest 객체의 필터링을 어떻게 수행할 지에 대한 설계 문서 
published: true
date: 2024-04-12T08:27:20.810Z
tags: 
editor: markdown
dateCreated: 2024-03-28T03:53:26.139Z
---

# 개요

이 문서는 POI 필터링 기능에서 사용하는 구조체와 구현 방식을 설명한다. 

## 상세

-   간단히 요약하자면 다음과 같은 위계 구조를 지닌다.

-   BP_POI_Manager
    -   BP_Filter 
        -   BP_TagCondition [0, ....]
        -   BP_NumericCondition [0, ...]
-   WBP_AttributeSearch 
    -   WBP_AttributeFilter_Tag [0, ...]
    -   WBP_AttributeFitler_Range[0, ...]

-   Filter Descirption Table 
    -   Filter Description 1
    -   Filter Description 2
    -   Filter Description 3
    -   Filter Description 4


-   POI Attribute Table 
    -   POI 1 Attribute
    -   POI 2 Attribute
    -   POI 3 Attribute
    -   POI 4 Attribute
    -   ....
    -   POI 100 Attribute

-   위 위계 구조를 가진 두 테이블을 이용해 Filter 위젯을 생성하는 순서는 다음과 같다
    1.  POI Info Manager에 두 데이터 테이블을 등록한다.
    2.  WBP_AttributeSearch 를 생성한다.
    3.  PIM(POI Info Manager)에 등록된 Filter Descriptions를 기반으로 Filter Widget을 생성한다.
    4.  Filter 위젯은 Filter Description에 따라 초기화 한다. 
        1.  이때 절차적 필터 생성이 일어난다.
        2.  필터링 오퍼레이션은 Filter 위젯의 소관이 아니며, Filter 위젯은 단순히 POI 매니저에 있는  Filter의 컨디션을 조정하는 기능만을 수행할 수 있다. 
따라서 책임 분리가 확실하게 수행된다.





자료구조

-   POI Filter Descriptor는 다음과 같다.
```cpp 
        enum class ETagAttributes
        {...
        };
        
        enum class ENumericAttributes
        {...    
        };
        
        USTRUCT()
        struct FFilterDescriptor
        {
            UPROPERTY()
            TSubclassOf<Class Of Filter Widget> FilterWidgetClass;
            
            UPROPERTY()
            FText AttributeLabel;
            
            UPROPERTY()
            TArray<FGameplayTag> TagAttributes;
            
            UPROPERTY()
        	TArray<float> NumericAttributes;
            // below lines for Nuemrical Attribute Filter
            UPROPERTY()
            FText AttributePostfix; // may be unused via Tag Attributes
            
            UPROPERTY()
            float Step;
            
            UPROPERTY()
            float DefaultValue;
        };
```
-   POI Attribute는 다음과 같다.
```
        USTRUCT()
        struct FPOIAttributes
        {
            UPROPERTY()
            TMap<ETagAttributes, FGameplayTag> TagAttributes;
            
            UPROPERTY()
            TMap<ENuemricAttributes, float> NumericAttributes;    
        };
        ```
