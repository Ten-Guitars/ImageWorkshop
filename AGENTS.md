# AGENTS.md

This file contains instructions for agentic coding assistants working on the ImageWorkshop codebase.

## Build, Lint, and Test Commands

```bash
# Install dependencies
composer install

# Run all tests
vendor/bin/phpunit

# Run single test file
vendor/bin/phpunit tests/ImageWorkshopTest.php

# Run single test method
vendor/bin/phpunit --filter testInitFromPath

# Fix code style (PSR-2)
vendor/bin/php-cs-fixer fix --config=.php-cs-fixer.php .

# Check code style without fixing
vendor/bin/php-cs-fixer fix --config=.php-cs-fixer.php --dry-run --diff .

# Run static analysis (PHPStan level 5)
vendor/bin/phpstan analyse

# Run rector for automated refactoring
vendor/bin/rector process
```

## Code Style Guidelines

### Formatting
- PSR-2 coding standard
- 4 spaces indentation (no tabs)
- LF line endings
- UTF-8 encoding
- Trim trailing whitespace
- Insert final newline at end of files

### Naming Conventions
- **Classes**: PascalCase (`ImageWorkshop`, `ImageWorkshopLayer`)
- **Methods**: camelCase (`addLayer`, `resizeInPixel`, `fixOrientation`)
- **Constants**: UPPER_CASE with underscores (`ERROR_NOT_AN_IMAGE_FILE`, `UNIT_PIXEL`)
- **Properties**: camelCase (`$width`, `$height`, `$layers`)
- **Variables**: camelCase (`$layerPositionX`, `$containerWidth`)
- **Parameters**: camelCase with descriptive names

### Types and Type Hints
- **Always** include type hints on all parameters and return types
- Use scalar types: `int`, `string`, `bool`, `float`, `array`
- Use `GdImage` for GD resource type hints (PHP 8.0+)
- Use union types where appropriate: `int|bool`, `int|float`
- Use nullable types: `?string`, `?int`
- Use array shapes in PHPDoc for structured arrays:
  ```php
  @return array{x: int, y: int}
  @return array{R: int, G: int, B: int}
  ```
- Use `self` for return types referencing the same class

### Imports and Namespaces
- Root namespace: `PHPImageWorkshop\`
- Class imports sorted alphabetically
- Use `use GdImage;` for GD resource types
- No leading backslash in use statements
- Keep use statements at top of file after namespace declaration

### Error Handling
- Define error codes as class constants
- Throw custom exceptions extending `ImageWorkshopBaseException`
- Include descriptive error messages with context
- Use exceptions, not error codes, for control flow
- Example exception: `throw new ImageWorkshopException('File not found', static::ERROR_IMAGE_NOT_FOUND);`

### Documentation
- **Always** include PHPDoc blocks for classes and public methods
- Describe purpose, parameters, and return values
- Include `@throws` for exceptions that may be thrown
- Example:
  ```php
  /**
   * Initialize a layer from a given image path
   *
   * @throws ImageWorkshopException
   */
  public static function initFromPath(string $path): ImageWorkshopLayer
  ```

### Testing
- Tests located in `tests/` directory following PSR-4: `PHPImageWorkshop\Tests\`
- Extend `PHPUnit\Framework\TestCase`
- Test method naming: `test` + method name being tested (`testInitFromPath`)
- Use `expectException()` for expected exceptions
- Use `markTestSkipped()` for conditional test execution
- Keep tests focused on single behaviors
- Use descriptive assertion messages

### Project Structure
- `src/` - Main source code
- `tests/` - PHPUnit tests
- `src/Core/` - Core classes (`ImageWorkshopLayer`, `ImageWorkshopLib`)
- `src/Exception/` - Exception classes
- `fixtures/` - Test assets and resources

### Additional Guidelines
- Use `array()` syntax, not `[]` (PSR-2)
- Default parameter values: use `null` for optional objects, empty arrays for collections
- Boolean parameters: use `true`/`false` explicitly
- Use `sprintf()` for string interpolation with variables
- Validate input early in methods
- Constants should be `public const` for external access, `protected const` for internal use
