git remote add origin https://github.com/rannlangel/readme-edit.git
git branch -M master
git push -u origin master
// Inject this script into a WebView or browser console to spoof WebGL renderer as Adreno 840
(function() {
    const spoofRenderer = "Qualcomm Adreno (TM) 840";
    const spoofVendor = "Qualcomm";
    const spoofVersion = "OpenGL ES 3.2 V@540.0 (GIT@abcdef)";

    const getParameter = WebGLRenderingContext.prototype.getParameter;
    WebGLRenderingContext.prototype.getParameter = function(parameter) {
        // WebGL debug info extension
        if (parameter === this.RENDERER) {
            return spoofRenderer;
        }
        if (parameter === this.VENDOR) {
            return spoofVendor;
        }
        if (parameter === this.VERSION) {
            return spoofVersion;
        }
        return getParameter.apply(this, arguments);
    };

    // For WEBGL_debug_renderer_info extension
    const getExtension = WebGLRenderingContext.prototype.getExtension;
    WebGLRenderingContext.prototype.getExtension = function(name) {
        const ext = getExtension.apply(this, arguments);
        if (name === "WEBGL_debug_renderer_info" && ext) {
            Object.defineProperty(ext, "UNMASKED_RENDERER_WEBGL", {
                get: () => "UNMASKED_RENDERER_WEBGL",
            });
            Object.defineProperty(ext, "UNMASKED_VENDOR_WEBGL", {
                get: () => "UNMASKED_VENDOR_WEBGL",
            });

            const getParameter = this.getParameter;
            this.getParameter = function(parameter) {
                if (parameter === ext.UNMASKED_RENDERER_WEBGL) {
                    return spoofRenderer;
                }
                if (parameter === ext.UNMASKED_VENDOR_WEBGL) {
                    return spoofVendor;
                }
                return getParameter.apply(this, arguments);
            };
        }
        return ext;
    };
})();
