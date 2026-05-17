# Contributing to Axiom Security

Thank you for your interest in contributing to Axiom Security! 🛡️

We welcome contributions from the community to make this cybersecurity app even better.

---

## 🤝 How to Contribute

### Reporting Bugs

**Before submitting a bug report:**
1. Check if the bug has already been reported in [Issues](https://github.com/yourusername/axiom-security/issues)
2. Ensure you're using the latest version
3. Gather relevant information:
   - Android version
   - Device model
   - Steps to reproduce
   - Expected vs actual behavior
   - Screenshots/logs if applicable

**Submit a bug report:**
1. Open a new issue with label `bug`
2. Use the bug report template
3. Provide detailed information
4. Include LogCat output if available

### Suggesting Features

**Before suggesting a feature:**
1. Check the [roadmap](CHANGELOG.md#unreleased) for planned features
2. Search existing feature requests
3. Consider if it aligns with the app's mission

**Submit a feature request:**
1. Open a new issue with label `enhancement`
2. Describe the feature clearly
3. Explain the use case
4. Consider implementation details

### Code Contributions

**Setting up development environment:**
```bash
# Clone the repository
git clone https://github.com/yourusername/axiom-security.git
cd axiom-security

# Open in Android Studio
# File → Open → Select axiom directory

# Sync Gradle dependencies
# Build → Sync Project with Gradle Files
```

**Before starting work:**
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Discuss major changes in an issue first

**Code style:**
- Follow [Kotlin coding conventions](https://kotlinlang.org/docs/coding-conventions.html)
- Use meaningful variable names
- Add comments for complex logic
- Keep functions small and focused
- Follow MVVM architecture pattern

**Commit guidelines:**
```bash
# Good commit messages
feat: Add voice passphrase authentication
fix: Resolve camera permission crash on Android 13
docs: Update USER_GUIDE with new features
refactor: Optimize phishing detection algorithm

# Conventional Commits format
<type>: <description>

Types: feat, fix, docs, style, refactor, test, chore
```

**Pull request process:**
1. Update documentation if needed
2. Add/update tests if applicable
3. Ensure code compiles without warnings
4. Test on physical device
5. Submit PR with clear description
6. Link related issues

### Documentation

Documentation improvements are always welcome:
- Fix typos or unclear explanations
- Add examples or tutorials
- Improve code comments
- Translate to other languages

### Testing

Help us improve quality:
- Write unit tests for new features
- Report edge cases
- Test on different devices/Android versions
- Provide performance benchmarks

---

## 🏗️ Development Guidelines

### Project Structure
```
app/src/main/java/com/example/axiom/
├── data/           # Database models, DAOs
├── services/       # Core detection services
├── viewmodel/      # MVVM state management
├── ui/screens/     # Compose UI screens
├── receivers/      # Broadcast receivers
└── MainActivity.kt # Entry point
```

### Adding a New Feature

**Example: Add new threat type**

1. **Update data model** (`data/ThreatLog.kt`):
```kotlin
enum class ThreatType {
    PHISHING_LINK,
    SUSPICIOUS_IMAGE,
    FINANCIAL_FRAUD,
    UNAUTHORIZED_ACCESS,
    MALICIOUS_APP,
    SUSPICIOUS_PERMISSION,
    NEW_THREAT_TYPE  // Add here
}
```

2. **Create service** (`services/NewFeatureService.kt`):
```kotlin
class NewFeatureService(private val context: Context) {
    fun detectThreat(): ThreatResult {
        // Implementation
    }
}
```

3. **Update ViewModel** (`viewmodel/SecurityViewModel.kt`):
```kotlin
val newFeatureService = NewFeatureService(application)

fun checkNewThreat() {
    // Add logic
}
```

4. **Add UI** (`ui/screens/NewFeatureScreen.kt`):
```kotlin
@Composable
fun NewFeatureScreen() {
    // Compose UI
}
```

5. **Update navigation** in `MainActivity.kt`

6. **Add tests** in test directory

7. **Update documentation** (README.md, USER_GUIDE.md)

### Code Review Checklist

Before submitting PR, verify:
- [ ] Code follows Kotlin conventions
- [ ] No hardcoded strings (use resources)
- [ ] Proper error handling
- [ ] Memory leaks checked
- [ ] Permissions handled gracefully
- [ ] Works offline (if applicable)
- [ ] Voice alerts added (if threat detection)
- [ ] Logged to database (if threat)
- [ ] UI responsive and accessible
- [ ] Documentation updated
- [ ] CHANGELOG.md updated

---

## 🔒 Security Considerations

**When contributing:**
- Never commit API keys or credentials
- Use secure coding practices
- Validate all inputs
- Handle permissions properly
- Test with security in mind
- Report vulnerabilities privately

**Reporting security vulnerabilities:**
Email security@axiomsecurity.com (do NOT open public issue)

---

## 📝 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

## 🌟 Recognition

Contributors will be:
- Listed in CHANGELOG.md credits
- Mentioned in release notes
- Added to CONTRIBUTORS.md (if created)

---

## 💬 Communication

- **GitHub Issues**: Bug reports, feature requests
- **Pull Requests**: Code contributions
- **Discussions**: Questions, ideas (if enabled)
- **Email**: support@axiomsecurity.com

---

## 🎯 Good First Issues

Looking for where to start? Check issues labeled:
- `good first issue`
- `help wanted`
- `documentation`

---

## ⚡ Quick Contribution Ideas

**Easy contributions:**
- Fix typos in documentation
- Add code comments
- Improve error messages
- Add translations
- Create test cases

**Medium contributions:**
- Optimize existing algorithms
- Add new UI themes
- Improve accessibility
- Enhance voice alerts
- Add new detection patterns

**Advanced contributions:**
- Implement AI/ML improvements
- Add new security features
- Performance optimizations
- Architecture improvements
- Integration with other services

---

## 📚 Resources

**Learning Android Development:**
- [Android Developer Docs](https://developer.android.com/)
- [Kotlin Documentation](https://kotlinlang.org/docs/home.html)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)

**Project-specific:**
- [README.md](README.md) - Architecture overview
- [USER_GUIDE.md](USER_GUIDE.md) - Feature descriptions
- [TESTING_GUIDE.md](TESTING_GUIDE.md) - Testing procedures

---

## ❓ Questions?

Not sure how to contribute? Have questions?
- Open a discussion
- Email support@axiomsecurity.com
- Check existing issues and PRs

---

**Thank you for making Axiom Security better! 🙏**

Together, we can create a safer digital world. 🛡️
