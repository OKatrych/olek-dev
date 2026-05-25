---
title: "Compose Multiplatform themed @Preview"
publishDate: 2025-08-11
description: "How to preview both dark and light themes side by side in Compose Multiplatform, working around the missing uiMode parameter."
tags: [kotlin, compose-multiplatform, android]
---

Compose Multiplatform's `@Preview` annotation recently got an update with support for parameters like `name`, `group`, `widthDp`, `heightDp`, `locale`, `showBackground`, and `backgroundColor`.

However, there is still no way to specify a dark or light theme — unlike Android's `uiMode` parameter.

## The solution

Taking inspiration from the KotlinConf app's `PreviewHelper` approach, we can create a wrapper composable that displays content twice — once with a dark theme and once with a light theme.

For full-screen previews that don't fit side-by-side, we can define a custom annotation that doubles the width:

```kotlin
@Preview(
    heightDp = PhoneScreenHeightDp,
    widthDp = PhoneScreenWidthDp * 2,
)
annotation class ScreenPreview
```

Then we create a wrapper composable that renders both themes:

```kotlin
@Composable
fun ScreenPreviewWrapper(
    content: @Composable () -> Unit,
) {
    Row {
        AppTheme(darkTheme = false) {
            PreviewContent(
                content = content,
                modifier = Modifier.width(PhoneScreenWidthDp.dp),
            )
        }
        AppTheme(darkTheme = true) {
            PreviewContent(
                content = content,
                modifier = Modifier.width(PhoneScreenWidthDp.dp),
            )
        }
    }
}
```

This way you get both light and dark theme previews side by side in a single preview window. The trick is setting the preview width to double the phone screen width so both versions fit comfortably.

If you have a different approach to handling themed previews in Compose Multiplatform, I'd love to hear about it!
