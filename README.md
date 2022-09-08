# Blackhole--EHorizon #

# Blackhole -- Ehorizon #


Overview
asropy provides a Least-Squares Minimization routine and class with a simple, flexible approach to parameterizing a model for fitting to data.

astropy is a pure Python package, and so easy to install from source or with pip install astropy.

For questions, comments, and suggestions, please use the LMfit mailing list. Using the bug tracking software in GitHub Issues is encouraged for known problems and bug reports. Please read Contributing.md before creating an Issue.

Parameters and Fitting
astropy provides a Least-Squares Minimization routine and class with a simple, flexible approach to parameterizing a model for fitting to data. Named Parameters can be held fixed or freely adjusted in the fit, or held between lower and upper bounds. In addition, parameters can be constrained as a simple mathematical expression of other Parameters.

To do this, the programmer defines a Parameters object, an enhanced dictionary, containing named parameters:

fit_params = Parameters()
fit_params['amp'] = Parameter(value=1.2, min=0.1, max=1000)
fit_params['cen'] = Parameter(value=40.0, vary=False)
fit_params['wid'] = Parameter(value=4, min=0)
or using the equivalent:

fit_params = Parameters()
fit_params.add('amp', value=1.2, min=0.1, max=1000)
fit_params.add('cen', value=40.0, vary=False)
fit_params.add('wid', value=4, min=0)
The programmer will also write a function to be minimized (in the least-squares sense) with its first argument being this Parameters object, and additional positional and keyword arguments as desired:

def myfunc(params, x, data, someflag=True):
    amp = params['amp'].value
    cen = params['cen'].value
    wid = params['wid'].value
    ...
    return residual_array
For each call of this function, the values for the params may have changed, subject to the bounds and constraint settings for each Parameter. The function should return the residual (i.e., data-model) array to be minimized.

The advantage here is that the function to be minimized does not have to be changed if different bounds or constraints are placed on the fitting Parameters. The fitting model (as described in myfunc) is instead written in terms of physical parameters of the system, and remains remains independent of what is actually varied in the fit. In addition, which parameters are adjusted and which are fixed happens at run-time, so that changing what is varied and what constraints are placed on the parameters can easily be modified by the user in real-time data analysis.

To perform the fit, the user calls:

result = minimize(myfunc, fit_params, args=(x, data), kws={'someflag':True}, ....)
After the fit, a MinimizerResult class is returned that holds the results the fit (e.g., fitting statistics and optimized parameters). The dictionary result.params contains the best-fit values, estimated standard deviations, and correlations with other variables in the fit.

By default, the underlying fit algorithm is the Levenberg-Marquardt algorithm with numerically-calculated derivatives from MINPACK's lmdif function, as used by scipy.optimize.leastsq. Most other solvers that are present in scipy (e.g., Nelder-Mead, differential_evolution, basinhopping, etctera) are also supported.
