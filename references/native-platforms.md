# Transparent Glass UI — Native Platforms Reference

Implementation guide for SwiftUI, Jetpack Compose, and React Native. All platforms follow the same core principles: dark surfaces, bright content, desaturated luminous accents, ambient motion, and glanceable typography.

## Table of Contents
1. [Design Tokens (Cross-Platform)](#design-tokens)
2. [SwiftUI](#swiftui)
3. [Jetpack Compose](#jetpack-compose)
4. [React Native](#react-native)
5. [Platform Comparison Matrix](#comparison)

---

## Design Tokens

These semantic values are the same across all platforms. Map them to platform-native types.

```
Surface:
  primary:    black @ 55% opacity    (main card background)
  elevated:   #0F0F14 @ 75% opacity  (raised cards, modals)
  overlay:    black @ 85% opacity    (full-screen overlays)

Content:
  text-1:     white @ 95% opacity    (primary text)
  text-2:     white @ 60% opacity    (secondary text)
  text-3:     white @ 35% opacity    (labels, captions)

Accents (desaturated, high-luminance):
  cyan:       #A0E6F5
  coral:      #FFBEB4
  gold:       #FFE1A0
  lilac:      #D2BEFF
  mint:       #B8F0D8
  rose:       #F5B8D0

Blur:
  standard:   24px radius, 1.2x saturation
  heavy:      40px radius, 1.4x saturation

Radius:
  small:      8pt / 8dp
  medium:     16pt / 16dp
  large:      24pt / 24dp

Motion:
  ambient:    1.2s ease-out (entrances, notifications)
  responsive: 0.15s ease-out (taps, input feedback)
  notification-enter: 1.8s ease-out (slow materialization)

Typography:
  Minimum weight: Medium (500)
  Letter spacing: +0.02em to +0.08em
  Minimum body size: 16pt / 16sp
```

---

## SwiftUI

SwiftUI has first-class support for glass effects through its material system, making it the most natural platform for transparent glass UI — especially on visionOS.

### Theme Setup

```swift
import SwiftUI

// MARK: - Glass Design Tokens

enum GlassTheme {
    // MARK: Surfaces
    static let surfacePrimary = Color.black.opacity(0.55)
    static let surfaceElevated = Color(red: 0.06, green: 0.06, blue: 0.08).opacity(0.75)
    static let surfaceOverlay = Color.black.opacity(0.85)
    
    // MARK: Content
    static let textPrimary = Color.white.opacity(0.95)
    static let textSecondary = Color.white.opacity(0.60)
    static let textTertiary = Color.white.opacity(0.35)
    
    // MARK: Accents — desaturated, luminous
    static let accentCyan = Color(red: 0.63, green: 0.90, blue: 0.96)
    static let accentCoral = Color(red: 1.0, green: 0.745, blue: 0.706)
    static let accentGold = Color(red: 1.0, green: 0.88, blue: 0.63)
    static let accentLilac = Color(red: 0.82, green: 0.75, blue: 1.0)
    static let accentMint = Color(red: 0.72, green: 0.94, blue: 0.84)
    static let accentRose = Color(red: 0.96, green: 0.72, blue: 0.82)

    // MARK: Borders
    static let borderSubtle = Color.white.opacity(0.08)
    static let borderBright = Color.white.opacity(0.15)
    
    // MARK: Shadows
    static let shadowColor = Color.black.opacity(0.45)
    
    // MARK: Radius
    static let radiusSmall: CGFloat = 8
    static let radiusMedium: CGFloat = 16
    static let radiusLarge: CGFloat = 24
    
    // MARK: Spacing
    static let spacingXs: CGFloat = 8
    static let spacingSm: CGFloat = 12
    static let spacingMd: CGFloat = 20
    static let spacingLg: CGFloat = 32
}
```

### Glass Card

```swift
struct GlassCard<Content: View>: View {
    let elevated: Bool
    let content: () -> Content
    
    init(elevated: Bool = false, @ViewBuilder content: @escaping () -> Content) {
        self.elevated = elevated
        self.content = content
    }
    
    var body: some View {
        content()
            .padding(GlassTheme.spacingLg)
            .frame(maxWidth: 480)
            .background {
                // Use native material on supported platforms
                if #available(iOS 15.0, *) {
                    RoundedRectangle(cornerRadius: GlassTheme.radiusMedium)
                        .fill(.ultraThinMaterial)
                        .overlay {
                            RoundedRectangle(cornerRadius: GlassTheme.radiusMedium)
                                .fill(elevated ? GlassTheme.surfaceElevated : GlassTheme.surfacePrimary)
                        }
                } else {
                    RoundedRectangle(cornerRadius: GlassTheme.radiusMedium)
                        .fill(elevated ? GlassTheme.surfaceElevated : GlassTheme.surfacePrimary)
                }
            }
            .clipShape(RoundedRectangle(cornerRadius: GlassTheme.radiusMedium))
            .overlay(
                RoundedRectangle(cornerRadius: GlassTheme.radiusMedium)
                    .stroke(GlassTheme.borderSubtle, lineWidth: 1)
                    .overlay(alignment: .top) {
                        // Brighter top edge — "glass edge catch"
                        Rectangle()
                            .fill(GlassTheme.borderBright)
                            .frame(height: 1)
                            .clipShape(RoundedRectangle(cornerRadius: GlassTheme.radiusMedium))
                    }
            )
            .shadow(color: GlassTheme.shadowColor, radius: elevated ? 24 : 16, y: elevated ? 16 : 8)
    }
}
```

### Glass Button

```swift
struct GlassButton: View {
    let title: String
    let variant: Variant
    let action: () -> Void
    
    enum Variant { case primary, ghost }
    
    init(_ title: String, variant: Variant = .primary, action: @escaping () -> Void) {
        self.title = title
        self.variant = variant
        self.action = action
    }
    
    var body: some View {
        Button(action: action) {
            Text(title)
                .font(.system(size: 13, weight: .semibold))
                .tracking(0.5) // letter-spacing
                .textCase(.uppercase)
                .padding(.horizontal, 20)
                .padding(.vertical, 10)
                .foregroundColor(variant == .primary ? GlassTheme.accentCyan : GlassTheme.textSecondary)
                .background(
                    RoundedRectangle(cornerRadius: GlassTheme.radiusSmall)
                        .fill(variant == .primary ? Color.white.opacity(0.12) : Color.clear)
                )
                .overlay(
                    RoundedRectangle(cornerRadius: GlassTheme.radiusSmall)
                        .stroke(
                            variant == .primary
                                ? GlassTheme.accentCyan.opacity(0.25)
                                : GlassTheme.borderSubtle,
                            lineWidth: 1
                        )
                )
        }
        .buttonStyle(.plain)
    }
}
```

### Materialize Animation

```swift
struct MaterializeModifier: ViewModifier {
    let delay: Double
    @State private var appeared = false
    
    func body(content: Content) -> some View {
        content
            .opacity(appeared ? 1 : 0)
            .scaleEffect(appeared ? 1 : 0.97)
            .offset(y: appeared ? 0 : 6)
            .blur(radius: appeared ? 0 : 6)
            .animation(
                .easeOut(duration: 1.2).delay(delay),
                value: appeared
            )
            .onAppear { appeared = true }
    }
}

extension View {
    func glassMaterialize(delay: Double = 0) -> some View {
        modifier(MaterializeModifier(delay: delay))
    }
}

// Usage:
// GlassCard { ... }.glassMaterialize(delay: 0.15)
```

### Glass Label / Status

```swift
struct GlassLabel: View {
    let text: String
    
    var body: some View {
        Text(text)
            .font(.system(size: 11, weight: .semibold))
            .tracking(1.0)
            .textCase(.uppercase)
            .foregroundColor(GlassTheme.textTertiary)
    }
}

struct GlassStatusDot: View {
    enum Status { case active, warning, error }
    let status: Status
    
    var color: Color {
        switch status {
        case .active: return GlassTheme.accentMint
        case .warning: return GlassTheme.accentGold
        case .error: return GlassTheme.accentCoral
        }
    }
    
    @State private var pulsing = false
    
    var body: some View {
        HStack(spacing: 6) {
            Circle()
                .fill(color)
                .frame(width: 6, height: 6)
                .shadow(color: color.opacity(0.5), radius: 4)
                .opacity(pulsing ? 0.4 : 1)
                .animation(.easeInOut(duration: 2).repeatForever(), value: pulsing)
                .onAppear { pulsing = true }
            
            Text(statusText)
                .font(.system(size: 11, weight: .semibold))
                .tracking(1.0)
                .textCase(.uppercase)
                .foregroundColor(color)
        }
    }
    
    var statusText: String {
        switch status {
        case .active: return "Active"
        case .warning: return "Warning"
        case .error: return "Error"
        }
    }
}
```

### visionOS Specifics

On visionOS, prefer system glass materials over manual colors:

```swift
// visionOS — use the system glassBackgroundEffect
struct VisionGlassCard<Content: View>: View {
    let content: () -> Content
    
    var body: some View {
        content()
            .padding(GlassTheme.spacingLg)
            .frame(maxWidth: 480)
            .glassBackgroundEffect() // System glass on visionOS
            .clipShape(RoundedRectangle(cornerRadius: GlassTheme.radiusMedium))
    }
}
```

---

## Jetpack Compose

Compose uses `Modifier.background()` with `Brush` and `BlendMode` for glass effects. On Android XR, Jetpack Compose Glimmer provides native glass components.

### Theme Setup

```kotlin
package com.example.glassui.theme

import androidx.compose.ui.graphics.Color
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

object GlassTheme {
    // Surfaces
    val surfacePrimary = Color.Black.copy(alpha = 0.55f)
    val surfaceElevated = Color(0xFF0F0F14).copy(alpha = 0.75f)
    val surfaceOverlay = Color.Black.copy(alpha = 0.85f)
    
    // Content
    val textPrimary = Color.White.copy(alpha = 0.95f)
    val textSecondary = Color.White.copy(alpha = 0.60f)
    val textTertiary = Color.White.copy(alpha = 0.35f)
    
    // Accents — desaturated, high-luminance
    val accentCyan = Color(0xFFA0E6F5)
    val accentCoral = Color(0xFFFFBEB4)
    val accentGold = Color(0xFFFFE1A0)
    val accentLilac = Color(0xFFD2BEFF)
    val accentMint = Color(0xFFB8F0D8)
    
    // Borders
    val borderSubtle = Color.White.copy(alpha = 0.08f)
    val borderBright = Color.White.copy(alpha = 0.15f)
    
    // Radius
    val radiusSmall = 8.dp
    val radiusMedium = 16.dp
    val radiusLarge = 24.dp
    
    // Spacing
    val spacingXs = 8.dp
    val spacingSm = 12.dp
    val spacingMd = 20.dp
    val spacingLg = 32.dp
}
```

### Glass Card

```kotlin
import androidx.compose.animation.core.*
import androidx.compose.foundation.background
import androidx.compose.foundation.border
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.blur
import androidx.compose.ui.draw.clip
import androidx.compose.ui.draw.shadow
import androidx.compose.ui.graphics.graphicsLayer
import androidx.compose.ui.unit.dp

@Composable
fun GlassCard(
    modifier: Modifier = Modifier,
    elevated: Boolean = false,
    animationDelay: Int = 0,
    content: @Composable ColumnScope.() -> Unit
) {
    val shape = RoundedCornerShape(GlassTheme.radiusMedium)
    
    // Materialize animation
    var appeared by remember { mutableStateOf(false) }
    val animProgress by animateFloatAsState(
        targetValue = if (appeared) 1f else 0f,
        animationSpec = tween(
            durationMillis = 1200,
            delayMillis = animationDelay,
            easing = FastOutSlowInEasing
        ),
        label = "materialize"
    )
    
    LaunchedEffect(Unit) { appeared = true }
    
    Column(
        modifier = modifier
            .graphicsLayer {
                alpha = animProgress
                scaleX = 0.97f + 0.03f * animProgress
                scaleY = 0.97f + 0.03f * animProgress
                translationY = (1f - animProgress) * 18f
            }
            .widthIn(max = 480.dp)
            .shadow(
                elevation = if (elevated) 24.dp else 16.dp,
                shape = shape,
                ambientColor = Color.Black.copy(alpha = 0.5f),
                spotColor = Color.Black.copy(alpha = 0.5f)
            )
            .clip(shape)
            .background(
                if (elevated) GlassTheme.surfaceElevated
                else GlassTheme.surfacePrimary
            )
            .border(1.dp, GlassTheme.borderSubtle, shape)
            .padding(GlassTheme.spacingLg),
        content = content
    )
}
```

### Glass Button

```kotlin
import androidx.compose.material3.Text
import androidx.compose.foundation.clickable
import androidx.compose.foundation.interaction.MutableInteractionSource

@Composable
fun GlassButton(
    text: String,
    modifier: Modifier = Modifier,
    variant: GlassButtonVariant = GlassButtonVariant.Primary,
    onClick: () -> Unit
) {
    val shape = RoundedCornerShape(GlassTheme.radiusSmall)
    
    Box(
        modifier = modifier
            .clip(shape)
            .background(
                when (variant) {
                    GlassButtonVariant.Primary -> Color.White.copy(alpha = 0.12f)
                    GlassButtonVariant.Ghost -> Color.Transparent
                }
            )
            .border(
                1.dp,
                when (variant) {
                    GlassButtonVariant.Primary -> GlassTheme.accentCyan.copy(alpha = 0.25f)
                    GlassButtonVariant.Ghost -> GlassTheme.borderSubtle
                },
                shape
            )
            .clickable(
                interactionSource = remember { MutableInteractionSource() },
                indication = null,
                onClick = onClick
            )
            .padding(horizontal = 20.dp, vertical = 10.dp),
        contentAlignment = Alignment.Center
    ) {
        Text(
            text = text.uppercase(),
            color = when (variant) {
                GlassButtonVariant.Primary -> GlassTheme.accentCyan
                GlassButtonVariant.Ghost -> GlassTheme.textSecondary
            },
            fontSize = 13.sp,
            fontWeight = FontWeight.SemiBold,
            letterSpacing = 0.5.sp
        )
    }
}

enum class GlassButtonVariant { Primary, Ghost }
```

### Glass Label / Typography

```kotlin
@Composable
fun GlassLabel(text: String, modifier: Modifier = Modifier) {
    Text(
        text = text.uppercase(),
        modifier = modifier,
        color = GlassTheme.textTertiary,
        fontSize = 11.sp,
        fontWeight = FontWeight.SemiBold,
        letterSpacing = 1.sp
    )
}

// Recommended text styles
object GlassTypography {
    val title = TextStyle(
        fontWeight = FontWeight.Bold,
        fontSize = 28.sp,
        letterSpacing = 0.5.sp,
        color = GlassTheme.textPrimary
    )
    val body = TextStyle(
        fontWeight = FontWeight.Medium,
        fontSize = 17.sp,
        letterSpacing = 0.3.sp,
        lineHeight = 26.sp,
        color = GlassTheme.textSecondary
    )
    val label = TextStyle(
        fontWeight = FontWeight.SemiBold,
        fontSize = 11.sp,
        letterSpacing = 1.sp,
        color = GlassTheme.textTertiary
    )
    val dataValue = TextStyle(
        fontWeight = FontWeight.Bold,
        fontSize = 40.sp,
        letterSpacing = (-0.5).sp,
        color = GlassTheme.textPrimary,
        fontFeatureSettings = "tnum" // tabular numbers
    )
}
```

### Android XR / Compose Glimmer

On Android XR, use Jetpack Compose Glimmer components directly. The theme above aligns with Glimmer's design philosophy. If building for Android XR glasses specifically, follow the official Glimmer design kit and use `SpatialPanel` for layout:

```kotlin
// Android XR — Spatial panel with Glimmer styling
SpatialPanel {
    GlassCard {
        Text("Navigation", style = GlassTypography.label)
        Text("N Shoreline Blvd", style = GlassTypography.title)
    }
}
```

---

## React Native

React Native requires a blur library for the frosted glass effect. Use `@react-native-community/blur` or `expo-blur`.

### Theme Setup

```typescript
// theme/glass.ts

export const GlassTheme = {
  surface: {
    primary: 'rgba(0, 0, 0, 0.55)',
    elevated: 'rgba(15, 15, 20, 0.75)',
    overlay: 'rgba(0, 0, 0, 0.85)',
  },
  text: {
    primary: 'rgba(255, 255, 255, 0.95)',
    secondary: 'rgba(255, 255, 255, 0.60)',
    tertiary: 'rgba(255, 255, 255, 0.35)',
  },
  accent: {
    cyan: '#A0E6F5',
    coral: '#FFBEB4',
    gold: '#FFE1A0',
    lilac: '#D2BEFF',
    mint: '#B8F0D8',
    rose: '#F5B8D0',
  },
  border: {
    subtle: 'rgba(255, 255, 255, 0.08)',
    bright: 'rgba(255, 255, 255, 0.15)',
  },
  radius: {
    sm: 8,
    md: 16,
    lg: 24,
  },
  spacing: {
    xs: 8,
    sm: 12,
    md: 20,
    lg: 32,
  },
} as const;
```

### Glass Card

```tsx
// components/GlassCard.tsx

import React, { useEffect, useRef } from 'react';
import { View, StyleSheet, Animated, Platform } from 'react-native';
import { BlurView } from '@react-native-community/blur';
// or: import { BlurView } from 'expo-blur';

import { GlassTheme } from '../theme/glass';

interface GlassCardProps {
  children: React.ReactNode;
  elevated?: boolean;
  delay?: number;
}

export const GlassCard: React.FC<GlassCardProps> = ({
  children,
  elevated = false,
  delay = 0,
}) => {
  const opacity = useRef(new Animated.Value(0)).current;
  const scale = useRef(new Animated.Value(0.97)).current;
  const translateY = useRef(new Animated.Value(6)).current;

  useEffect(() => {
    Animated.parallel([
      Animated.timing(opacity, {
        toValue: 1,
        duration: 1200,
        delay,
        useNativeDriver: true,
        easing: (t) => 1 - Math.pow(1 - t, 3), // ease-out cubic
      }),
      Animated.timing(scale, {
        toValue: 1,
        duration: 1200,
        delay,
        useNativeDriver: true,
      }),
      Animated.timing(translateY, {
        toValue: 0,
        duration: 1200,
        delay,
        useNativeDriver: true,
      }),
    ]).start();
  }, []);

  return (
    <Animated.View
      style={[
        styles.card,
        elevated && styles.cardElevated,
        {
          opacity,
          transform: [{ scale }, { translateY }],
        },
      ]}
    >
      {/* Blur background — platform specific */}
      {Platform.OS === 'ios' ? (
        <BlurView
          style={StyleSheet.absoluteFill}
          blurType="dark"
          blurAmount={24}
          reducedTransparencyFallbackColor={
            elevated ? 'rgba(15,15,20,0.92)' : 'rgba(0,0,0,0.88)'
          }
        />
      ) : (
        // Android fallback — solid dark surface (blur is expensive)
        <View
          style={[
            StyleSheet.absoluteFill,
            {
              backgroundColor: elevated
                ? GlassTheme.surface.elevated
                : GlassTheme.surface.primary,
            },
          ]}
        />
      )}

      {/* Content layer */}
      <View style={styles.cardContent}>{children}</View>
    </Animated.View>
  );
};

const styles = StyleSheet.create({
  card: {
    maxWidth: 480,
    borderRadius: GlassTheme.radius.md,
    overflow: 'hidden',
    borderWidth: 1,
    borderColor: GlassTheme.border.subtle,
    // iOS shadow
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 8 },
    shadowOpacity: 0.45,
    shadowRadius: 16,
    // Android shadow
    elevation: 12,
  },
  cardElevated: {
    shadowOffset: { width: 0, height: 16 },
    shadowOpacity: 0.6,
    shadowRadius: 24,
    elevation: 20,
  },
  cardContent: {
    padding: GlassTheme.spacing.lg,
  },
});
```

### Glass Button

```tsx
// components/GlassButton.tsx

import React from 'react';
import { TouchableOpacity, Text, StyleSheet } from 'react-native';
import { GlassTheme } from '../theme/glass';

interface GlassButtonProps {
  title: string;
  variant?: 'primary' | 'ghost';
  onPress: () => void;
}

export const GlassButton: React.FC<GlassButtonProps> = ({
  title,
  variant = 'primary',
  onPress,
}) => {
  const isPrimary = variant === 'primary';

  return (
    <TouchableOpacity
      onPress={onPress}
      activeOpacity={0.7}
      style={[
        styles.button,
        {
          backgroundColor: isPrimary ? 'rgba(255,255,255,0.12)' : 'transparent',
          borderColor: isPrimary
            ? 'rgba(160,230,245,0.25)'
            : GlassTheme.border.subtle,
        },
      ]}
    >
      <Text
        style={[
          styles.buttonText,
          { color: isPrimary ? GlassTheme.accent.cyan : GlassTheme.text.secondary },
        ]}
      >
        {title.toUpperCase()}
      </Text>
    </TouchableOpacity>
  );
};

const styles = StyleSheet.create({
  button: {
    paddingHorizontal: 20,
    paddingVertical: 10,
    borderRadius: GlassTheme.radius.sm,
    borderWidth: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  buttonText: {
    fontSize: 13,
    fontWeight: '600',
    letterSpacing: 0.5,
  },
});
```

### Glass Status + Label

```tsx
// components/GlassStatus.tsx

import React, { useEffect, useRef } from 'react';
import { View, Text, Animated, StyleSheet } from 'react-native';
import { GlassTheme } from '../theme/glass';

type Status = 'active' | 'warning' | 'error';

const STATUS_COLORS: Record<Status, string> = {
  active: GlassTheme.accent.mint,
  warning: GlassTheme.accent.gold,
  error: GlassTheme.accent.coral,
};

export const GlassStatusDot: React.FC<{ status: Status }> = ({ status }) => {
  const pulse = useRef(new Animated.Value(1)).current;
  const color = STATUS_COLORS[status];

  useEffect(() => {
    Animated.loop(
      Animated.sequence([
        Animated.timing(pulse, { toValue: 0.4, duration: 1000, useNativeDriver: true }),
        Animated.timing(pulse, { toValue: 1, duration: 1000, useNativeDriver: true }),
      ])
    ).start();
  }, []);

  return (
    <View style={styles.statusRow}>
      <Animated.View style={[styles.dot, { backgroundColor: color, opacity: pulse }]} />
      <Text style={[styles.statusText, { color }]}>{status.toUpperCase()}</Text>
    </View>
  );
};

export const GlassLabel: React.FC<{ text: string }> = ({ text }) => (
  <Text style={styles.label}>{text.toUpperCase()}</Text>
);

const styles = StyleSheet.create({
  statusRow: { flexDirection: 'row', alignItems: 'center', gap: 6 },
  dot: { width: 6, height: 6, borderRadius: 3 },
  statusText: { fontSize: 11, fontWeight: '600', letterSpacing: 1 },
  label: {
    fontSize: 11,
    fontWeight: '600',
    letterSpacing: 1,
    color: GlassTheme.text.tertiary,
  },
});
```

### Expo Blur Alternative

If using Expo, the blur setup is simpler:

```tsx
import { BlurView } from 'expo-blur';

// Inside GlassCard:
<BlurView intensity={60} tint="dark" style={StyleSheet.absoluteFill} />
```

---

## Platform Comparison Matrix

| Feature | Web (CSS) | SwiftUI | Compose | React Native |
|---------|-----------|---------|---------|--------------|
| **Blur** | `backdrop-filter` | `.ultraThinMaterial` | Custom (RenderEffect) | `@react-native-community/blur` |
| **Blur quality** | Excellent | Native, best | Limited on older APIs | iOS excellent, Android fallback |
| **Dark surfaces** | `rgba()` | `Color.opacity()` | `Color.copy(alpha=)` | `rgba()` string |
| **Borders** | `border` CSS | `.overlay` + `stroke` | `Modifier.border()` | `borderWidth` + `borderColor` |
| **Shadows** | `box-shadow` | `.shadow()` | `Modifier.shadow()` | `shadowColor/Offset/Radius` + `elevation` |
| **Materialize anim** | CSS `@keyframes` | `withAnimation` | `animateFloatAsState` | `Animated.timing` |
| **Ambient motion** | CSS transitions | `.animation(.easeOut(duration: 1.2))` | `tween(1200ms)` | `Animated.timing(1200)` |
| **Status pulsing** | CSS `@keyframes pulse` | `.repeatForever()` | `infiniteRepeatable` | `Animated.loop` |
| **visionOS support** | N/A | `.glassBackgroundEffect()` | N/A | N/A |
| **Android XR** | N/A | N/A | Compose Glimmer native | N/A |
| **Reduced motion** | `prefers-reduced-motion` | `UIAccessibility.isReduceMotionEnabled` | `Settings.Global.ANIMATOR_DURATION_SCALE` | `AccessibilityInfo.isReduceMotionEnabled()` |

### Key Platform Notes

- **iOS/SwiftUI**: Best native glass support. Use `.ultraThinMaterial` or `.regularMaterial` as the base, then overlay your dark surface color. On visionOS use `.glassBackgroundEffect()`.
- **Android/Compose**: Native blur is expensive pre-API 31. On older devices, fall back to solid dark surfaces — the design still works because the core principle is dark surfaces + bright content, not blur. On Android XR, use Compose Glimmer directly.
- **React Native**: iOS gets great blur via `BlurView`. Android gets a solid dark fallback. The design holds up without blur because contrast comes from the color system, not transparency.
- **All platforms**: The 6 design principles (dark surfaces, additive color, glanceable type, depth via shadow, ambient motion, bright focus states) work regardless of whether blur is available. Blur is an enhancement, not the foundation.
