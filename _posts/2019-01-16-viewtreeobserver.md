---
layout: post
title: "ViewTreeObserver를 통한 아이템 갯수 제한"
categories: [Android]
tags: [Android]
---

# ViewTreeObserver

[ViewTreeObserver](https://developer.android.com/reference/android/view/ViewTreeObserver)는 뷰 트리의 글로벌 변경을 통지받을 수 있는 리스너를 등록하는데 사용된다. 이를 통해 각종 뷰의 최종 크기를 알 수 있다.

# ChipGroup에 화면 사이즈만큼 아이템 수 제한 

최초 아이템을 담을 컨테이너의 가로 크기를 ViewTreeObserver를 통해 구한다. 화면의 가로 크기와 같다.

```java
private int chipGroupWidth = 0;
private void setupChips(FinderResult $data) {
    chipGroup.removeAllViews();
    chipGroup.getViewTreeObserver().addOnGlobalLayoutListener(()->{
        chipGroupWidth = chipGroup.getWidth();
    });
    for (JsonElement jsonElement : $data.getData) {
      String name = jsonElement.getAsJsonObject().get("name").getAsString();
      MyChip chip = createMyChip(name);
      itemContainer.addView(chip); 
      // addView 이후에야 MyChip 가로길이를 onGloablLayout를 통해 구할 수 있다.
    }
}

```

다음으로, 추가되는 아이템의 가로길이를 ViewTreeObserver를 통해 구한다. 그리고, 아이템 가로길이를 구한 후, 사용한 리스너를 제거하기 위해 tag에 onGlobalLayoutListener를 저장한다.(setTag)

```java
private MyChip createMyChip(String name) {
	MyChip chip = new MyChip(itemView.getContext());
    ViewTreeObserver.OnGlobalLayoutListener chipLayoutListener = () -> onChipLayout(chip);
    chip.setTag(chipLayoutListener);
    chip.getViewTreeObserver().addOnGlobalLayoutListener(chipLayoutListener);
    chip.setChecked(false);
    chip.setEnabled(false);
    chip.setText(name);
    return chip;
}
```

ViewTreeObserver를 아이템 하나하나에 등록해준 것은 아이템의 가로길이만을 구하려고 한 것이다. 때문에 바로 지워줄 필요가 있다. 만약 지워주지 않는다면 메모리릭이 발생한다. 그리고, onGlobalLayout() 메서드가 n번 호출되는 상황에 맞닥드린다.

```java
private int totalChipSize = 0; 
...
...
private void onChipLayout(MyChip chip) {
    // 등록했던 OnGlobalLayoutListener 제거하기기기기
	chip.getViewTreeObserver().removeOnGlobalLayoutListener(getChipLayoutListener(chip));
	totalChipSize += chip.getWidth();
	if (totalChipSize > itemContainerWidth && itemContainer.getChildCount() > 0) {
		int removeIndex = itemContainer.getChildCount() - 1;
      	chip.removeViewAt(removeIndex);
        // addView로 추가됐던 아이템을 지워준다. 또는 보이지 않게 해주는 것도 괜찮다.
	}
}

```

```java
// 단순히, chip tag에 저장해놨던 onGlobalLayoutListener를 가져온다.
private ViewTreeObserver.OnGlobalLayoutListener getChipLayoutListener(MyChip chip) {
	return (ViewTreeObserver.OnGlobalLayoutListener) chip.getTag();
}
```

# 결론

chipGroup에 chip이 addView 된 이후에만 onGlobalLayoutListener를 통해 chip의 크기를 알 수 있다. 때문에 성능상에 문제가 있다. 이 문제를 피하려면 onMeasure통한 방법도 있을것 같다.

아이템 갯수가 아주 많아지면 onGlobalLayoutListener를 사용하는 방법은 좋지 않을 수 있다.