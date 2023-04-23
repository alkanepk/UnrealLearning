# How to add custom overlay icon to asset thumbnail

1. Create c++ class derived from `FAssetTypeActions_Base`
2. Implement virtual funciton from `IAssetTypeActions`

```cpp
#pragma once

#include "CoreMinimal.h"
#include "AssetTypeActions_Base.h"
#include "Data/CustomDataAsset.h"



class MYPROJECTEDITOR_API FCustomDataAssetActions : public FAssetTypeActions_Base
{

public:
	// IAssetTypeActions Implementation
	virtual FText GetName() const override { return NSLOCTEXT("AssetTypeActions", "AssetTypeActions_CustomDataAsset", "CustomDataAsset"); };
	virtual FColor GetTypeColor() const override {return FColor(0,255,190);};
	virtual UClass* GetSupportedClass() const override { return UCustomDataAsset::StaticClass(); };
	virtual uint32 GetCategories() override { return EAssetTypeCategories::Misc; };
	virtual TSharedPtr<SWidget> GetThumbnailOverlay(const FAssetData& AssetData) const override;
	

};
```
3. Implement thumbnail overlay in cpp file

```cpp
#include "CustomDataAssetActions.h"
#include "CustomDataAsset.h"
#include "Styling/SlateIconFinder.h"


TSharedPtr<SWidget> FVEVfxDataAssetActions::GetThumbnailOverlay(const FAssetData& AssetData) const
{
	const FSlateBrush* Icon = FSlateIconFinder::FindIconBrushForClass(UCustomDataAsset::StaticClass());

	return SNew(SBorder)
		.BorderImage(FAppStyle::GetNoBrush())
		.Visibility(EVisibility::HitTestInvisible)
		.Padding(FMargin(0.0f, 0.0f, 0.0f, 3.0f))
		.HAlign(HAlign_Right)
		.VAlign(VAlign_Bottom)
		[
			SNew(SImage)
			.Image(Icon)
		];
}

```
