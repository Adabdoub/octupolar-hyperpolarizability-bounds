# Contributing to Octupolar Hyperpolarizability Research

Thank you for your interest in contributing to this project! We welcome contributions from researchers, students, and practitioners in mathematical physics, quantum chemistry, and nonlinear optics.

## Code of Conduct

We are committed to providing a welcoming and inspiring community for all. Please read and follow our [Code of Conduct](#code-of-conduct-summary).

---

## How to Contribute

There are many ways to contribute to this project:

### 1. **Report Bugs**

If you find a bug, please open a [GitHub Issue](https://github.com/Adabdoub/octupolar-hyperpolarizability/issues) with:

- A clear, descriptive title
- Exact steps to reproduce the bug
- Expected behavior vs. actual behavior
- Your Python version, OS, and relevant package versions
- Any error messages or traceback

Example:
```
Title: KKT solver diverges for N=8 tetrahedral configuration

Steps:
1. Run: python validation/numerical_C_N.py --n 8
2. Solver fails to converge after 500 iterations

Expected: Should converge with C(8) ≈ 0.92
Actual: Diverges with NaN values
Environment: Python 3.9, NumPy 1.21.0, SciPy 1.7.1
```

### 2. **Request Features**

Suggest new features via [GitHub Issues](https://github.com/Adabdoub/octupolar-hyperpolarizability/issues) with:

- Clear description of the feature
- Why it would be useful
- Potential implementation approach (if you have ideas)
- Links to related issues or papers

Example:
```
Title: Add frequency-dependent (dynamic) hyperpolarizability extension

Description:
The current framework only handles static (ω=0) hyperpolarizabilities. 
Extending to dynamic cases would enable modeling of experimental HRS 
measurements at multiple wavelengths.

Implementation idea: Modify the resolvent operator R to include 
damping terms (complex energy denominators).

Related: Paper by Zyss (1994), citation #5 in references
```

### 3. **Improve Documentation**

Help make the documentation clearer, more accurate, and more complete:

- Fix typos or grammatical errors
- Clarify confusing sections
- Add examples or use cases
- Improve docstrings
- Add tutorials or notebooks

### 4. **Add Material Examples**

Contribute new material design examples with:

- Detailed structural and electronic properties
- Predicted hyperpolarizability values
- Experimental validation strategy
- Synthesis procedures (if applicable)

Example template in `materials/new_example.md`:
```markdown
# Tetraphenylarsenium Complexes

## Structure
- Core: AsV with four phenyl groups
- Symmetry: Td
- Electronic structure: ...

## Energy Levels
- State 1: 2.3 eV
- State 2: 2.4 eV
- State 3: 2.5 eV
- State 4: 2.6 eV

## Predicted β_oct
~1.5×10^5 a.u.

## Experimental Validation Protocol
HRS in THF at 1064 nm...
```

### 5. **Extend the Theory**

Contribute theoretical extensions:

- Frequency-dependent formulation
- 2nd hyperpolarizability (γ) bounds
- Relativistic corrections
- Environmental/solvent effects

### 6. **Create Computational Tools**

Develop new code contributions:

- Faster KKT solvers
- Visualization tools
- Machine learning surrogate models
- Database of precomputed C(N) values

---

## Getting Started with Development

### Fork the Repository

```bash
# Fork on GitHub (click the "Fork" button)

# Clone your fork
git clone https://github.com/YOUR-USERNAME/octupolar-hyperpolarizability.git
cd octupolar-hyperpolarizability

# Add upstream remote for syncing
git remote add upstream https://github.com/Adabdoub/octupolar-hyperpolarizability.git
```

### Set Up Development Environment

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install development dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt  # For testing and docs

# Install pre-commit hooks (optional, recommended)
pre-commit install
```

### Create a Feature Branch

```bash
# Sync with upstream
git fetch upstream
git rebase upstream/main

# Create feature branch
git checkout -b feature/my-contribution
# or for bug fixes:
git checkout -b fix/issue-description
```

---

## Development Workflow

### Code Style

We follow **PEP 8** conventions with Black formatter:

```bash
# Format code
black . --line-length 100

# Check style
flake8 . --max-line-length=100

# Type checking (optional)
mypy . --ignore-missing-imports
```

### Writing Tests

All contributions should include tests. Place tests in `tests/` directory:

```python
# tests/test_my_feature.py

import pytest
from octupolar.my_module import my_function

def test_basic_functionality():
    """Test basic behavior of my_function."""
    result = my_function(input_value=42)
    assert result == expected_value

def test_edge_cases():
    """Test edge cases."""
    with pytest.raises(ValueError):
        my_function(invalid_input=-1)

def test_numerical_accuracy():
    """Test against known analytical result."""
    result = my_function(test_case="known_good")
    assert abs(result - analytical_value) < 1e-10
```

Run tests locally:

```bash
# Run all tests
pytest tests/

# Run specific test file
pytest tests/test_my_feature.py -v

# Run with coverage
pytest tests/ --cov=octupolar --cov-report=html
```

### Documentation

Add docstrings to all functions following NumPy style:

```python
def compute_C_N(n_levels, n_configs=100):
    """Compute variational constant C(N) for a given level number.
    
    This function optimizes the hyperpolarizability tensor subject to
    the TRK constraint and octupolar constraint, returning the ratio
    of achievable octupolar response to the Kuzyk bound.
    
    Parameters
    ----------
    n_levels : int
        Number of quantum levels (N ≥ 3).
    n_configs : int, optional
        Number of random initial configurations to try (default: 100).
    
    Returns
    -------
    C_N : float
        Variational constant C(N) ∈ [0, 1].
    result : dict
        Detailed optimization results including energy levels,
        transition dipoles, and achieved hyperpolarizability.
    
    Raises
    ------
    ValueError
        If n_levels < 3.
    
    Notes
    -----
    The computation uses KKT conditions with SQP optimization.
    For N ≥ 20, computation may be slow; consider parallelization.
    
    Examples
    --------
    >>> C_4, result = compute_C_N(4)
    >>> print(f"C(4) = {C_4:.4f}")  # Output: C(4) = 0.6124
    
    References
    ----------
    .. [1] Theorem 1 in Dabdoub (2026), equations (15)-(18)
    """
```

### Commit Messages

Write clear, descriptive commit messages:

```
# Good
git commit -m "Add KKT solver for optimization

- Implement Karush-Kuhn-Tucker conditions
- Support inequality constraints on energy gaps
- Add unit tests with 10 test cases
- Achieves 0.15% numerical error vs analytical solution

Fixes #42"

# Avoid
git commit -m "fix bug"
git commit -m "update code"
```

### Submitting a Pull Request

1. **Rebase and squash** (if multiple commits for single feature):
   ```bash
   git rebase -i upstream/main
   ```

2. **Push to your fork**:
   ```bash
   git push origin feature/my-contribution
   ```

3. **Create Pull Request on GitHub**:
   - Give a clear title: "Add frequency-dependent hyperpolarizability support"
   - Reference related issues: "Closes #42"
   - Describe your changes and motivation
   - Include any new dependencies

4. **PR Checklist** (in PR description):
   ```markdown
   - [ ] Tests pass locally (`pytest tests/`)
   - [ ] Code style checked (`black . && flake8 .`)
   - [ ] Documentation updated
   - [ ] Docstrings added to new functions
   - [ ] CHANGELOG.md updated
   - [ ] No new dependencies without discussion
   ```

---

## Priority Contribution Areas

Help is especially needed in these areas:

### 🔴 High Priority
- [ ] Dynamic (frequency-dependent) hyperpolarizability extension
- [ ] Performance optimization for N > 30
- [ ] Documentation improvements and tutorials
- [ ] Additional material design examples (metals, organics, 2D materials)

### 🟡 Medium Priority
- [ ] Machine learning surrogate models for design
- [ ] Visualization tools for tensor decomposition
- [ ] Extension to second hyperpolarizability (γ)
- [ ] Solvent/environment effects

### 🟢 Low Priority
- [ ] Additional unit tests
- [ ] Code refactoring
- [ ] Minor documentation fixes

---

## Code Review Process

After you submit a PR:

1. **Automated checks run**: GitHub Actions will run tests and style checks
2. **Author review**: The maintainer will review your code for:
   - Correctness of implementation
   - Alignment with project goals
   - Code quality and style
   - Documentation completeness
   - Numerical accuracy
3. **Feedback loop**: You may be asked to make changes
4. **Approval and merge**: Once approved, your PR will be merged to `main`

---

## Attribution and Recognition

All contributors will be:

- Listed in the CONTRIBUTORS.md file
- Acknowledged in merged commit messages
- Mentioned in the project README

---

## Code of Conduct Summary

### Be Respectful
- Treat all contributors with respect
- Welcome different perspectives and backgrounds
- Assume good intentions

### Be Inclusive
- Use inclusive language
- Welcome contributions from all skill levels
- Help others learn and grow

### Be Professional
- Provide constructive feedback
- Focus on ideas, not personalities
- Communicate clearly and kindly

### Unacceptable Behavior
- Harassment, discrimination, or offensive language
- Personal attacks or insults
- Spam or deliberately disruptive behavior
- Sharing others' private information

Violations will result in removal from the project.

---

## Questions?

- **Documentation**: Check [README.md](README.md) and inline code comments
- **GitHub Issues**: Ask in relevant issue or create a new one
- **Email**: Contact the maintainer at [abdullah.dabdoub@gmail.com](mailto:abdullah.dabdoub@gmail.com)

---

## Additional Resources

- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
- [NumPy Documentation Style Guide](https://numpydoc.readthedocs.io/en/latest/format.html)
- [Black Code Formatter](https://github.com/psf/black)

---

## Thank You!

We truly appreciate all contributions, big and small. Your work helps advance the field of nonlinear optics and makes this research accessible to the broader community.

**Happy coding! 🚀**

---

**Version:** 1.0.0  
**Last Updated:** June 2026  
**Maintained by:** Abdullah M. Dabdoub
